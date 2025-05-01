# Git Cheat Sheet for Salesforce Deployments

This cheat sheet provides common Git commands used when developing and deploying Salesforce metadata changes, often managed via platforms like GitHub.

## 1. Common Branching Strategies

Different teams adopt various branching models. Here are a few common ones:

* **Promotion-Based Branching / Environment Promotion Flow / Stage-Based Branching:**
    * **Concept:** Branches directly represent deployment stages or environments. Common examples include `develop` (or `int`), `uat`, and `main` (or `prod`). These are typically long-lived branches.
    * **Detailed Flow Example (`develop` -> `uat` -> `prod`):**
        1.  **Feature Development:** Create a new short-lived `feature/<task-name>` branch based off the `develop` branch.
            ```bash
            git checkout develop
            git pull origin develop
            git checkout -b feature/SFDX-123-NewFeature develop
            ```
        2.  **Develop & Commit:** Make changes, commit them to the feature branch.
            ```bash
            # ...make code changes...
            git add .
            git commit -m "feat: Implement new feature (SFDX-123)"
            git push origin feature/SFDX-123-NewFeature
            ```
        3.  **Merge to Develop:** Create a Pull Request (PR) from `feature/SFDX-123-NewFeature` to `develop`. Once approved and tested (perhaps via CI deployment to a scratch org or dev sandbox), merge it.
        4.  **Deploy to Integration (INT):** The `develop` branch is deployed to the INT sandbox/environment for integration testing.
        5.  **Promote to UAT:** When ready for UAT, create a PR from `develop` to `uat`. Review and merge.
            ```bash
            # Example commands (often done via PR interface)
            # git checkout uat
            # git pull origin uat
            # git merge develop --no-ff -m "Merge develop into uat for Release X.Y"
            # git push origin uat
            ```
        6.  **Deploy to UAT:** The `uat` branch is deployed to the UAT sandbox/environment for user acceptance testing.
        7.  **Promote to Production:** Once UAT is signed off, create a PR from `uat` to `prod` (or `main`). Review carefully and merge. This merge often triggers the production deployment.
            ```bash
            # Example commands (often done via PR interface)
            # git checkout prod
            # git pull origin prod
            # git merge uat --no-ff -m "Merge uat into prod for Release X.Y"
            # git push origin prod
            ```
        8.  **Deploy to Production:** The `prod` (or `main`) branch is deployed to the Production environment.
        9.  **(Optional) Tag Release:** Tag the merge commit on `prod`/`main` with a release version.
            ```bash
            git tag -a v1.2.0 <commit_hash_on_prod> -m "Release 1.2.0"
            git push origin v1.2.0
            ```
        10. **(Optional) Merge Back (if needed):** Sometimes changes (like hotfixes done directly on a `hotfix` branch off `prod`) need to be merged back down (`prod` -> `uat`, `prod` -> `develop`) to keep environments consistent.
    * **Characteristics:** Clear mapping between branches and environments. Can lead to long-lived branches and potentially complex merges if features diverge significantly. Requires discipline in managing promotions.

* **Trunk-Based Development:**
    * **Concept:** All developers work off a single main branch (often called `trunk` or `main`).
    * **Flow:**
        * Small changes might be committed directly to `main`.
        * Larger features are built on short-lived feature branches that branch off `main` and are merged back quickly (within hours or a day or two).
    * **Characteristics:** Relies heavily on comprehensive automated testing and often uses feature flags to hide incomplete work in production. Aims to reduce merge conflicts and keep the main branch always releasable.

* **Gitflow (More Complex):** While not explicitly requested, it's another common pattern involving `main`, `develop`, `feature`, `release`, and `hotfix` branches. It's often considered overly complex for many Salesforce projects compared to simpler promotion or trunk-based models.

*(The choice of branching strategy depends on team size, release cadence, complexity, and tooling.)*

---

## 2. Initial Setup & Branching

* **Clone the Repository:** Get a local copy of the project.
    ```bash
    git clone <repository_url>
    ```
    *(Replace `<repository_url>` with the actual URL from GitHub/GitLab/etc.)*

* **Navigate into the Repository:**
    ```bash
    cd <repository_directory_name>
    ```

* **Ensure You're on the Main/Develop Branch:** (Depending on your strategy)
    ```bash
    # Example for trunk-based or simple promotion starting from main
    # git checkout main
    # git pull origin main

    # Example for Environment Promotion Flow or Gitflow starting from develop
    git checkout develop
    git pull origin develop
    ```

* **Create a New Feature Branch:** Create a dedicated branch for your task/user story (usually off `main` or `develop`).
    ```bash
    # Branching off main (Trunk-based)
    # git checkout -b feature/<your-task-name> main

    # Branching off develop (Environment Promotion Flow)
    git checkout -b feature/<your-task-name> develop
    ```
    *(Example: `git checkout -b feature/SFDX-123-AddAccountValidation develop`)*

## 3. Daily Development Workflow

* **Check Status:** See which files have been modified.
    ```bash
    git status
    ```

* **Add Changes:** Stage specific files or all changes for commit.
    * Add specific file(s):
        ```bash
        git add path/to/your/metadata/file.cls
        git add path/to/another/file.object
        ```
    * Add all changes in the current directory and subdirectories:
        ```bash
        git add .
        ```

* **Commit Changes:** Save your staged changes to the local branch history with a descriptive message.
    ```bash
    git commit -m "feat: Add validation rule for Account Name (SFDX-123)"
    ```
    *(Use clear commit messages, often referencing the task ID.)*

* **Push Branch to Remote:** Upload your local branch commits to the remote repository (e.g., GitHub).
    ```bash
    git push origin feature/<your-task-name>
    ```
    *(If it's the first push for this branch, you might need: `git push --set-upstream origin feature/<your-task-name>`)*

* **Fetch & Rebase (Optional but Recommended):** Keep your feature branch updated with the latest changes from the base branch (`develop` in this example) to avoid large merge conflicts later.
    ```bash
    # Fetch the latest changes from the remote base branch (e.g., develop)
    git fetch origin develop

    # Rebase your current branch onto the latest base branch
    git rebase origin/develop

    # You might need to resolve conflicts during rebase and then:
    # git add <conflicted_files>
    # git rebase --continue

    # Force push after rebase (use with caution, only on your own feature branch)
    git push origin feature/<your-task-name> --force-with-lease
    ```

## 4. Merging & Release (Environment Promotion Flow Example)

* **Create Pull Request (Feature -> Develop):** Via GitHub/GitLab interface, compare `feature/<your-task-name>` against `develop`. Review, test (CI), and merge.
* **Create Pull Request (Develop -> UAT):** When ready for UAT, compare `develop` against `uat`. Review and merge.
* **Create Pull Request (UAT -> Prod):** When UAT is approved, compare `uat` against `prod` (or `main`). Review carefully and merge for release.
* **Pull Changes Locally (After Merges):** Keep your local long-lived branches updated.
    ```bash
    git checkout develop
    git pull origin develop

    git checkout uat
    git pull origin uat

    git checkout prod # Or main
    git pull origin prod # Or main
    ```
* **Delete Local Feature Branch (Optional):**
    ```bash
    git branch -d feature/<your-task-name>
    ```
* **Delete Remote Feature Branch (Optional):** Often done via the web interface after merging the PR to `develop`.
    ```bash
    git push origin --delete feature/<your-task-name>
    ```

## 5. Reverting a Task (Before Release / After Merge to Develop/UAT)

This addresses how to undo commits associated with a specific task *before* those changes have been deployed/released to production, especially if the commits are already merged into a shared branch like `develop` or `uat`.

* **Identify the Commit(s) to Revert:** Find the hash(es) of the merge commit (or individual commits if preferred) on the branch you want to fix (e.g., `develop`).
    ```bash
    git checkout develop
    git log --oneline --graph
    ```
    *(Look for the merge commit related to your feature. Copy the unique commit hash (e.g., `a1b2c3d`).)*

* **Revert the Merge Commit:** This creates a *new* commit on `develop` that undoes the changes introduced by the merge.
    ```bash
    # On the branch where the bad merge occurred (e.g., develop)
    git revert -m 1 <merge_commit_hash_to_undo>
    ```
    *(Example: `git revert -m 1 a1b2c3d`). The `-m 1` typically refers to the mainline parent (the `develop` branch itself before the merge).*
    * Git will open a commit message editor. You can usually keep the default message and save/close.

* **Push the Revert Commit:** Push the newly created revert commit to the remote branch.
    ```bash
    git push origin develop
    ```
* **Important:** If the bad change was already promoted (e.g., merged from `develop` to `uat`), you need to repeat the revert process on the `uat` branch as well (reverting the `develop` -> `uat` merge commit).

**Why `git revert`?**

* **Preserves History:** It doesn't delete commits; it adds new ones that undo previous changes. This is crucial for shared branches.
* **Transparency:** Clearly shows in the history that a change was intentionally undone.
* **Safer:** Avoids rewriting history on shared branches (`git reset --hard`).

## 6. Useful Helper Commands

* **View Commit History:**
    ```bash
    git log
    git log --oneline --graph --decorate
    ```
* **Show Changes in a Commit:**
    ```bash
    git show <commit_hash>
    ```
* **Show Changes Between Branches:**
    ```bash
    git diff <branch1>..<branch2>
    ```
    *(Example: `git diff develop..uat` shows changes in `uat` since it diverged from `develop`)*
* **Stash Changes:** Temporarily save uncommitted changes.
    ```bash
    git stash push -m "WIP: My temporary changes"
    git stash list
    git stash apply stash@{0} # Apply the latest stash
    git stash drop stash@{0}  # Remove the latest stash after applying/checking
    ```
