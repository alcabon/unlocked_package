# unlocked_package

Okay, let's break down an optimal strategy for Salesforce DevOps using a hybrid approach: source-driven development in Dev orgs, transitioning to unlocked packages for Integration, UAT, and Production, all orchestrated with GitHub Actions.

This model attempts to balance developer speed in early stages with the robustness and modularity of packages for release management. However, as you noted, it presents challenges, primarily around ensuring consistency between the source-based Dev environment and the packaged deployments.

Core Strategy: Git as the Source of Truth, Package-Centric Releases

Source Control (Git/GitHub): This is non-negotiable. Your Git repository must be the absolute source of truth for all metadata, regardless of where development initially happens.
Repository Structure: Structure your Git repository like an SFDX project from the beginning, even if developers aren't building package versions initially. Define your package directories within sfdx-project.json.

```JSON

// sfdx-project.json example
{
  "packageDirectories": [
    { "path": "force-app/core", "default": false, "package": "CoreServices", "versionName": "ver 0.1", "versionNumber": "0.1.0.NEXT" },
    { "path": "force-app/sales-feature", "default": true, "package": "SalesFeature", "versionName": "ver 0.1", "versionNumber": "0.1.0.NEXT",
      "dependencies": [ {"package": "CoreServices", "versionNumber": "0.1.0.LATEST"} ]
    }
    // Add other package directories as needed
  ],
  "namespace": "", // Usually empty for unlocked packages
  "sfdcLoginUrl": "https://login.salesforce.com",
  "sourceApiVersion": "60.0", // Use the relevant API version
  "packageAliases": {
    "CoreServices": "0Ho...",
    "SalesFeature": "0Ho..."
    // Aliases get filled in when packages are created
  }
}
```

Development Process:
Developers work on feature branches checked out from a main development branch (e.g., develop).
They connect their local VS Code/SFDX CLI to a shared Dev sandbox or individual Dev orgs.
Crucially: Developers use sf project deploy start and sf project retrieve start (or VS Code extensions) to sync metadata between their local, correctly structured SFDX project (cloned from Git) and their assigned Dev org.
They commit changes to the appropriate package directory within their feature branch in Git.
Continuous Integration (CI) - Feature Branch/Pull Request:
When a feature branch is pushed or a Pull Request is opened against develop, a GitHub Action should trigger.
Action: Validate the metadata in the PR against a dedicated CI scratch org or sandbox. This catches syntax errors and basic deployment issues early. Run local Apex tests included in the committed source.
Integration - Merging to develop:
Upon merging a feature branch into develop, a GitHub Action triggers the core packaging and deployment process.
Action:
Build Beta Package(s): Create beta versions of the unlocked package(s) defined in sfdx-project.json from the develop branch source code.
Deploy to Integration: Install these beta package(s) into the Integration environment (a dedicated sandbox).
Run Tests: Execute Apex tests within the Integration sandbox (potentially a wider set of tests than in CI).
User Acceptance Testing (UAT) - Release Branch:
When develop is deemed ready for UAT, create a release/ branch (e.g., release/v1.2.0).
Action (Triggered on release/* branch creation/push):
Promote Package(s): Find the last successful beta package version(s) created from the commit lineage now in the release branch and promote them to released status. This locks the package version.
Deploy to UAT: Install the promoted (released) package version(s) into the UAT sandbox.
Run Tests: Execute relevant tests in UAT (Apex tests, potentially trigger automated UI tests).
Production Deployment - Merging to main:
After successful UAT sign-off, merge the release/* branch into main and tag the merge commit (e.g., v1.2.0).
Action (Triggered on merge to main or manual trigger):
Deploy to Production: Install the exact same promoted package version(s) that were tested in UAT into the Production org. Use the --target-org ProductionAlias.
Run Tests: Execute specified Apex tests (often RunLocalTests or a specific suite) in Production.
GitHub Actions Examples (Conceptual YAML)

You'll need Actions secrets for Dev Hub, Integration, UAT, and Prod org authentication (preferably using JWT Bearer Flow).

1. Validate PR (.github/workflows/validate-pr.yml)

```YAML

name: Validate PR Metadata

on:
  pull_request:
    branches: [ develop ]
    paths:
      - 'force-app/**' # Only run if Salesforce metadata changes

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0 # Needed for delta checks if you implement them

    - name: Install Salesforce CLI
      run: |
        npm install --global @salesforce/cli

    - name: Authenticate to CI Org (e.g., Scratch Org or Sandbox)
      run: |
        echo "${{ secrets.SFDX_CI_AUTH_URL }}" > ./SFDX_CI_URL.txt
        sf org login sfdx-url --sfdx-url-file ./SFDX_CI_URL.txt --alias ci-org --set-default

    - name: Validate Metadata Deployment (Quick Check)
      # Use deploy checkonly. Might need delta logic for faster checks on large repos.
      run: sf project deploy validate --source-dir force-app --target-org ci-org --test-level RunLocalTests

    # Optional: Add specific Apex test execution here if needed
2. Build Beta Package & Deploy to Integration (.github/workflows/deploy-integ.yml)
```

```YAML

name: Build Beta Package & Deploy to Integration

on:
  push:
    branches: [ develop ]
    paths:
      - 'force-app/**'

concurrency: # Prevent multiple concurrent builds on the same branch
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    - name: Install Salesforce CLI
      run: npm install --global @salesforce/cli

    - name: Authenticate to Dev Hub
      run: |
        echo "${{ secrets.SFDX_DEVHUB_AUTH_URL }}" > ./SFDX_DH_URL.txt
        sf org login sfdx-url --sfdx-url-file ./SFDX_DH_URL.txt --alias DevHub --set-default-dev-hub

    - name: Create Beta Package Version(s)
      id: create_package # Give step an ID to access outputs
      run: |
        # Example for one package 'SalesFeature'. Repeat/script for multiple.
        # Consider using JQ/Bash to parse sfdx-project.json for package names
        PACKAGE_NAME="SalesFeature"
        VERSION_INFO=$(sf package version create --package "$PACKAGE_NAME" --installation-key-bypass --wait 20 --code-coverage --version-name "beta-${{ github.sha }}" --tag "${{ github.sha }}" --branch "${{ github.ref_name }}" -x --json)
        echo "Created Package Version Info: $VERSION_INFO"
        PACKAGE_VERSION_ID=$(echo $VERSION_INFO | jq -r .result.SubscriberPackageVersionId)
        echo "PACKAGE_VERSION_ID=$PACKAGE_VERSION_ID" >> $GITHUB_OUTPUT # Store for next step

    - name: Authenticate to Integration Org
      run: |
        echo "${{ secrets.SFDX_INTEG_AUTH_URL }}" > ./SFDX_INTEG_URL.txt
        sf org login sfdx-url --sfdx-url-file ./SFDX_INTEG_URL.txt --alias integ-org

    - name: Install Beta Package in Integration
      run: |
        sf package install --package "${{ steps.create_package.outputs.PACKAGE_VERSION_ID }}" --wait 10 --publish-wait 10 --target-org integ-org

    - name: Run Apex Tests in Integration
      run: |
        # Be specific about which tests to run
        sf apex run test --test-level RunSpecifiedTests --class-names "MySalesFeatureTests,AnotherTestClass" --target-org integ-org --result-format human --synchronous
```

3. Promote & Deploy to UAT (.github/workflows/deploy-uat.yml)

```YAML

name: Promote Package & Deploy to UAT

on:
  push:
    branches: [ 'release/**' ] # Trigger on pushes to release branches

jobs:
  promote_and_deploy_uat:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    - name: Install Salesforce CLI
      run: npm install --global @salesforce/cli

    - name: Authenticate to Dev Hub
      run: |
          echo "${{ secrets.SFDX_DEVHUB_AUTH_URL }}" > ./SFDX_DH_URL.txt
          sf org login sfdx-url --sfdx-url-file ./SFDX_DH_URL.txt --alias DevHub --set-default-dev-hub

    - name: Promote Package Version(s)
      id: promote_package
      run: |
        # --- THIS IS THE TRICKY PART ---
        # You need a reliable way to find the LATEST BETA Package Version ID
        # created from the commit history now in this release branch.
        # Options:
        # 1. Use the tag set during beta creation (if you used --tag <commit_sha>)
        # 2. Query package versions using sf package version list and filter by branch/tag/commit
        # Example using a known beta ID (replace with dynamic lookup logic):
        BETA_PACKAGE_VERSION_ID="04t..." # Get this dynamically!
        echo "Promoting Package Version ID: $BETA_PACKAGE_VERSION_ID"
        sf package version promote --package $BETA_PACKAGE_VERSION_ID --no-prompt
        echo "PROMOTED_PACKAGE_VERSION_ID=$BETA_PACKAGE_VERSION_ID" >> $GITHUB_OUTPUT # Promoted ID is the same

    - name: Authenticate to UAT Org
      run: |
        echo "${{ secrets.SFDX_UAT_AUTH_URL }}" > ./SFDX_UAT_URL.txt
        sf org login sfdx-url --sfdx-url-file ./SFDX_UAT_URL.txt --alias uat-org

    - name: Install Promoted Package in UAT
      run: |
        sf package install --package "${{ steps.promote_package.outputs.PROMOTED_PACKAGE_VERSION_ID }}" --wait 10 --publish-wait 10 --target-org uat-org

    - name: Run Apex Tests in UAT
      run: |
        sf apex run test --test-level RunSpecifiedTests --class-names "MySalesFeatureTests,AnotherTestClass" --target-org uat-org --result-format human --synchronous # Adjust test level/classes for UAT
```

4. Deploy to Production (.github/workflows/deploy-prod.yml)

```YAML

name: Deploy Package to Production

on:
  # Option 1: Trigger automatically on merge to main (if release branch was source)
  # push:
  #   branches: [ main ]
  # Option 2: Manual trigger for safety
  workflow_dispatch:
    inputs:
      package_version_id:
        description: 'Promoted Package Version ID (04t...)'
        required: true

jobs:
  deploy_production:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3 # Might not be needed if only installing

    - name: Install Salesforce CLI
      run: npm install --global @salesforce/cli

    - name: Authenticate to Production Org
      run: |
        echo "${{ secrets.SFDX_PROD_AUTH_URL }}" > ./SFDX_PROD_URL.txt
        sf org login sfdx-url --sfdx-url-file ./SFDX_PROD_URL.txt --alias prod-org

    - name: Install Promoted Package in Production
      run: |
        # Use the ID from manual trigger or find it based on the Git tag
        PACKAGE_VERSION_ID="${{ github.event.inputs.package_version_id }}"
        echo "Installing Package ID: $PACKAGE_VERSION_ID"
        sf package install --package "$PACKAGE_VERSION_ID" --wait 10 --publish-wait 10 --security-type AdminsOnly --target-org prod-org # Adjust security type

    - name: Run Apex Tests in Production
      run: |
        # Usually RunLocalTests or specific suites validated in UAT
        sf apex run test --test-level RunLocalTests --target-org prod-org --result-format human --synchronous
```

Addressing the Challenges:

Consistency Dev vs. Package: This is the biggest hurdle.
Discipline: Developers must use sf project retrieve to pull changes into the correct package structure locally before committing. Avoid developing components directly in the Dev org UI without syncing back.
Validation: The CI validation step (sf project deploy validate) against a clean org is crucial to catch issues caused by dependencies or metadata existing in the Dev org but not committed to Git.
Dependency Definition: Be meticulous in defining package dependencies in sfdx-project.json. The packaging step (sf package version create) will fail if dependencies aren't met within the codebase being packaged.
Metadata Not Packageable: Maintain these components (e.g., Profiles, Queues, Groups, some Reports/Dashboards) in a separate directory (e.g., unpackaged-metadata). Add steps to your GitHub Actions (after package installation) to deploy this metadata using sf project deploy start --source-dir unpackaged-metadata --target-org <alias>. Manage this carefully, as it bypasses package versioning.
Finding the Right Beta Version to Promote: This requires robust logic. Using Git tags (--tag "${{ github.sha }}" during beta creation) and then querying based on the commit history of the release branch is a common approach. You might query sf package version list --verbose and filter using jq based on the tag or branch.
This hybrid strategy requires significant team discipline and robust automation to mitigate the risks of divergence between the development environment and the packaged artifacts. Starting development within the package structure from the beginning, possibly using scratch orgs, generally leads to a smoother process, although it might have a steeper initial learning curve.
