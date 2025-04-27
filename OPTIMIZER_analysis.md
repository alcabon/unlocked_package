# **Salesforce Optimizer Retirement and Free Tool Alternatives Analysis**

## **1\. Executive Summary**

This report details the planned retirement of the Salesforce Optimizer tool and provides an analysis of free alternative tools capable of performing similar organizational health checks. The Salesforce Optimizer application, which succeeded the earlier PDF report format, is confirmed for retirement in the Winter '26 release.1 The legacy PDF report was previously retired after June 2021\.3

The analysis concludes that no single free tool offers a direct, comprehensive replacement for the full range of checks previously provided by Salesforce Optimizer. However, a combination of existing free tools can cover a significant portion of its functionality. The primary free alternatives identified are:

* **Salesforce Health Check:** Primarily focused on security configuration settings.  
* **SFDX Scanner (Salesforce Code Analyzer):** Focused on static analysis of custom code (Apex, LWC, Aura).  
* **Lightning Flow Scanner:** Specialized in analyzing Salesforce Flows and declarative automation.  
* **Megalinter:** An orchestration tool capable of running multiple scanners (including SFDX Scanner and Flow Scanner) within CI/CD pipelines.

Achieving coverage comparable to the Optimizer necessitates adopting a multi-tool strategy. The retirement of the Optimizer, a broad but sometimes shallow analysis tool, appears to align with a broader trend towards more specialized, in-depth analysis tools within the Salesforce ecosystem. This shift requires Salesforce administrators, developers, and architects to adapt their maintenance and analysis workflows, moving from reliance on a single, consolidated report to integrating insights from multiple, focused tools. This report aims to guide professionals through this transition by providing a detailed comparison and outlining a strategic approach to utilizing these alternative free resources effectively.

## **2\. The Retirement of Salesforce Optimizer**

The Salesforce Optimizer tool has served as a valuable resource for administrators seeking insights into their org's health and potential areas for improvement. However, Salesforce has officially announced its phased retirement, necessitating a transition plan for users who rely on its analysis.

### **2.1. Retirement Timeline**

The retirement of Salesforce Optimizer is occurring in stages, affecting both its historical PDF format and the current in-app version:

* **PDF Report Retirement:** The original Salesforce Optimizer PDF report format was officially retired after June 2021\.3 This retirement coincided with the promotion of the interactive Salesforce Optimizer app, which offered more features and an actionable format compared to the static PDF.3  
* **Optimizer App Retirement:** The interactive Salesforce Optimizer app is scheduled for a complete retirement for all organizations in the **Winter '26** release.1 Leading up to this final date, access to the Optimizer app will be restricted in newly created Salesforce orgs. Specifically, orgs created after **March 31, 2025**, will not have access to the Salesforce Optimizer feature.1 Existing orgs created before this date can continue using the Optimizer app until their instance is upgraded to Winter '26.1 It is worth noting that prior to the official retirement announcement, the Optimizer app experienced periods of unavailability attributed to ongoing maintenance, which caused some confusion among users.5

### **2.2. Impact for Salesforce Professionals**

The deprecation of the Salesforce Optimizer presents several implications for professionals responsible for maintaining Salesforce orgs:

* **Need for Alternatives:** Organizations currently utilizing the Optimizer for regular health checks, pre-release validation, or identifying areas for improvement must identify and adopt alternative tools and processes before the Winter '26 deadline.1  
* **Loss of Centralized Overview:** Optimizer provided a single report covering a wide array of features, from security settings and code checks to user adoption and configuration limits.6 Its retirement means losing this consolidated view provided natively by Salesforce.  
* **Increased Analysis Effort:** Gathering a similar breadth of information will likely require using multiple specialized tools and potentially developing custom reports or manual checklists for areas not fully covered by free alternatives. This fragmentation may increase the time and effort needed to perform comprehensive org analysis.

The phased retirement approach, disabling the tool in new orgs first before removing it entirely, provides a transition window for existing users.1 This pattern is consistent with how Salesforce often manages feature retirements.8 However, it underscores the finality of the decision and the importance for organizations to begin evaluating and implementing replacement strategies well in advance of the Winter '26 release to avoid disruptions in their org monitoring and maintenance practices.

## **3\. Understanding Salesforce Optimizer's Scope**

To effectively identify replacements, it is essential to understand the specific objectives and the range of features analyzed by the Salesforce Optimizer. It served as a broad diagnostic tool aimed at improving overall org health and efficiency.

### **3.1. Core Objectives**

Salesforce Optimizer was designed to assist administrators and other Salesforce professionals by taking the guesswork out of org maintenance.9 Its primary goals included:

* **Improving Implementation:** Providing recommendations for better utilization of Salesforce features.10  
* **Cleaning Up Customizations:** Identifying unused or problematic customizations.7  
* **Reducing Complexity:** Highlighting areas where configurations could be simplified.7  
* **Driving Feature Adoption:** Pointing out underutilized features that could benefit users.7  
* **Maintaining Org Health:** Offering a periodic check-up on various aspects of the Salesforce instance.7

The tool functioned by analyzing an org's metadata and providing a personalized report containing advice and actionable recommendations.4 It did not analyze the actual data within records.6

### **3.2. Key Areas and Features Analyzed**

Salesforce Optimizer provided a uniquely broad scan across numerous facets of a Salesforce org within a single interface. While the specific list evolved over time, with some checks being removed due to low usage even before the final retirement 15, the core application analyzed a comprehensive set of features.6 Key areas included:

* **Configuration & Limits:**  
  * Active Rules: Sharing Rules, Validation Rules, Workflow Rules (including limits).  
  * Storage Limits: Data Storage, File Storage, Static Resource limits (often visualized with history graphs 13).  
  * Object Limits: Page Layouts per Object, Record Types per Object, Custom Fields per Object.  
  * Org Setup: My Domain configuration.  
* **Security & Access:**  
  * Permissions: Admin Permissions, Critical Permission Assignments (e.g., Modify All Data), Profile Assignments, Permission Set Assignments (including those with low user counts).  
  * Sharing & Visibility: Insecure Experience Cloud site Sharing Settings, Insecure Default External Access Levels, Sharing Rules for All External Users, Public Groups/Queues with Guest Users.  
  * Authentication: Multi-Factor Authentication (MFA) Adoption (with limitations noted for SSO 14).  
* **Code & Customization:**  
  * API Usage: API Versions being used in code, checks for new code using old API versions.  
  * Apex: Multiple Apex Triggers per Object.  
  * Formulas/Links: Formula Fields with JavaScript Code, Hard-Coded URLs.  
  * Legacy Components: S-Controls.  
  * Automation Migration: Recommendations for Migrating Workflow Rules to Flow Builder.  
* **User Interface & Adoption:**  
  * Lightning Experience: Lightning Components on Record Pages, Lightning Sales Console, Lightning Service Console, Path usage, Details on Record Pages.  
  * Classic UI Elements: Notes and Attachments Related List usage (vs. Files Adoption).  
  * Collaboration: Chatter Usage (Inactive Users, Incomplete Profiles).  
  * User Environment: Unsupported Browsers, Out-of-Date Browsers, User Logins.  
* **Data Management & Maintenance:**  
  * Data Quality: Duplicate Management setup.  
  * Field Optimization: Field Usage analysis, Fields on Page Layouts.  
  * Cleanup: Unused Dashboards, Unused Reports.  
  * Unassigned Metadata: Unassigned Custom Profiles, Unassigned Page Layouts, Unassigned Permission Sets, Unassigned Record Types, Unassigned Roles.

This extensive list 6 demonstrates the Optimizer's wide reach across configuration, security, code, UI, and maintenance. While the analysis in some areas might have been less deep than specialized tools, its strength lay in this breadth, offering a holistic starting point for org assessment. Replicating this scope requires understanding which alternative tools cover which specific domains.

## **4\. Free Tools for Salesforce Org Analysis**

With the retirement of Salesforce Optimizer, users must turn to other tools. Fortunately, several free options are available, each specializing in different aspects of org analysis. Understanding their capabilities is key to building a replacement strategy.

### **4.1. Salesforce Health Check**

* **Description:** Salesforce Health Check is a native Salesforce tool primarily designed to assess and improve an org's security posture.11 It operates directly within the Salesforce Setup menu.  
* **Capabilities:** The tool scans the org's security settings and compares them against Salesforce Baseline Standards or up to five imported custom baselines.17 It generates a percentage score (0-100%) reflecting compliance with the chosen baseline.17 Health Check categorizes identified vulnerabilities as High-Risk, Medium-Risk, Low-Risk, or Informational.17 For many settings, it offers a "Fix Risks" button allowing administrators to quickly adjust settings to meet the baseline recommendations directly from the Health Check page.17  
* **Key Checks:** Health Check focuses on critical security configurations, including Password Policies, Session Settings (timeouts, IP restrictions), Certificate and Key Management, Login Access Policies, Remote Site Settings, and guest user access configurations.17 It also checks MFA settings compliance against the baseline 17, although Optimizer had limitations checking MFA usage via SSO.14  
* **Availability:** Health Check is a free tool available in Professional, Enterprise, Performance, Unlimited, and Developer Editions.17 Access requires specific user permissions: 'View Health Check', 'Manage Health Check', or the broader 'View Security Center' or 'Manage Security Center' permissions.17

### **4.2. SFDX Scanner (Salesforce Code Analyzer)**

* **Description:** Salesforce Code Analyzer is distributed as a plugin for the Salesforce Command Line Interface (SFDX CLI).22 It acts as a unified static analysis tool, integrating multiple underlying scanning engines to examine source code.24  
* **Capabilities:** SFDX Scanner analyzes various code types including Apex, Lightning Web Components (LWC), Aura Components, and JavaScript.25 It leverages engines like PMD (for Apex and other languages), ESLint (for JavaScript, LWC, Aura), RetireJS (for outdated JavaScript libraries), PMD Copy Paste Detector (CPD), and the Salesforce Graph Engine (for dataflow analysis, often used in security checks).25 The tool identifies issues related to code quality, potential performance bottlenecks, security vulnerabilities, and adherence to coding best practices.24 Its rules are highly configurable, allowing teams to tailor the analysis to their specific standards, including adding custom PMD rules.27  
* **Key Checks (Examples via PMD for Apex** 28**):**  
  * *Security:* CRUD/FLS checks (ApexCRUDViolation), SOQL injection (ApexSOQLInjection), insecure Crypto usage (ApexBadCrypto), insecure endpoints (ApexInsecureEndpoint), sharing violations (ApexSharingViolations).  
  * *Performance:* SOQL/DML inside loops (AvoidSOQLInLoops, AvoidDMLStatementsInLoops), use of System.debug (AvoidDebugStatements).  
  * *Best Practices:* Unused local variables (UnusedLocalVariable), missing assertions in tests (ApexUnitTestClassShouldHaveAsserts).  
  * *Code Style:* Naming conventions, brace usage for loops/if statements.  
  * *Error Prone:* Empty blocks (catch, if, while), DML in constructors, hardcoded IDs.  
  * *Design:* Cyclomatic and cognitive complexity, excessive class/method length or parameters.  
* **Availability:** SFDX Scanner is a free plugin.25 Its use requires installing the Salesforce CLI 22 and typically a Java Development Kit (JDK version 8 or later).23 It is run from the command line, making it suitable for local developer use and integration into automated CI/CD pipelines.24

### **4.3. Lightning Flow Scanner**

* **Description:** This tool is specifically designed for static analysis of Salesforce's declarative automation tools: Lightning Flows, and historically Process Builder and Workflows.32 It is available through multiple interfaces: an SFDX CLI plugin, a Visual Studio Code extension, potentially a Salesforce native app (via AppExchange), and integrations with tools like Copado.32  
* **Capabilities:** Lightning Flow Scanner examines the metadata of flows to identify deviations from best practices, potential performance issues (especially concerning governor limits), maintainability problems, and configuration errors.32 It can help enforce standards and improve the quality of low-code automation.32  
* **Key Checks:** Common checks include identifying DML operations or SOQL queries inside loops, usage of hardcoded IDs, flows lacking descriptions or fault paths, flows using outdated API versions, adherence to naming conventions, detection of inactive flows, analysis of flows running in System Mode without Sharing, checks for inefficient same-record field updates, and flagging the use of soon-to-be-retired Process Builders.32 The ruleset is configurable, allowing severity adjustments and exceptions.33  
* **Availability:** The core engine, SFDX plugin, and VSCode extension are open-source and free.32 An AppExchange package called "Flow Analyzer" also exists, built upon similar principles and offered for free, providing a UI-based alternative within Salesforce.41 Using the plugin requires Salesforce CLI, while the extension requires VS Code.32

### **4.4. Megalinter**

* **Description:** Megalinter is not a scanner itself, but rather an open-source orchestrator designed to run a wide variety of linters and static analysis tools within a CI/CD environment.46 It bundles linters for numerous languages and formats, including those relevant to Salesforce development.26  
* **Capabilities:** Megalinter automates the execution of configured linters against repository sources during CI/CD workflows (e.g., on pull requests in GitHub Actions, GitLab CI, Jenkins).46 For Salesforce, it can be configured to run SFDX Scanner (for Apex, LWC, Aura), Lightning Flow Scanner, and various security scanners (like Checkov for infrastructure-as-code, Gitleaks/Secretlint/Trivy for secrets detection, Semgrep for general security patterns).26 It consolidates the results and can provide feedback directly in pull requests.46 It also includes linters for copy-paste detection, spelling, and repository configuration checks.47  
* **Key Checks (Orchestrated):** The specific checks depend on the linters enabled in its configuration. It effectively leverages the capabilities of the underlying tools like SFDX Scanner and Flow Scanner, adding value through automation, consolidation, and additional checks like secrets scanning and copy-paste detection.26  
* **Availability:** Megalinter is free and open-source.46 It requires setup within a CI/CD platform, typically involving configuration via a YAML file (e.g., .github/workflows/mega-linter.yml for GitHub Actions).48 Its configuration allows granular control over which linters run against which file types.48

The landscape of free tools presents specialized solutions rather than a single generalist tool like Optimizer. Health Check addresses org security settings, SFDX Scanner tackles custom code, Flow Scanner focuses on declarative automation, and Megalinter provides a framework for automating these checks within development pipelines. Consequently, a comprehensive analysis strategy must involve selecting and combining these tools based on the specific areas of the org under review.

## **5\. Mapping Optimizer Checks to Free Alternatives**

A primary challenge in transitioning away from Salesforce Optimizer is understanding how its diverse checks map to the capabilities of the available free tools. This section provides a direct comparison to guide users in selecting appropriate alternatives.

### **5.1. Detailed Comparison Table**

The following table maps major Salesforce Optimizer check categories and specific checks (derived primarily from 6) to the free tool(s) that offer the most relevant verification capabilities. It also includes notes on the extent of coverage and potential gaps.

| Optimizer Check Category | Specific Optimizer Check | Primary Free Tool(s) | Coverage Notes & Gaps |
| :---- | :---- | :---- | :---- |
| **Security & Access** | Multi-Factor Authentication Adoption | Health Check | Health Check verifies MFA *permission/setting* compliance against baseline.17 Optimizer noted limitations for SSO logins.14 Actual usage may require Login History review.19 |
|  | Admin Permissions | Health Check, Manual Review | Health Check reviews high-risk permissions against baseline. Detailed review of admin profiles/perm sets often manual. |
|  | Critical Permission Assignments | Health Check, Manual Review | Similar to Admin Permissions; Health Check flags baseline deviations. Specific assignments (e.g., Modify All Data) require targeted review. |
|  | Insecure Experience Cloud site Sharing Settings | Health Check | Health Check includes settings related to guest user access and Experience Cloud security.21 |
|  | Insecure Default External Access Levels | Health Check | Health Check reviews org-wide default sharing settings against baseline recommendations.17 |
|  | Sharing Rules for All External Users | Manual Review | No specific automated check in free tools identified. Requires manual review of sharing rules targeting 'All Internal/External Users'. |
|  | Public Groups and Queues with Guest Users | Manual Review / Custom Report | Requires checking group/queue members manually or via reporting. |
|  | Profile Assignments (Low \# Users) | Manual Report | Requires custom report on User Profile assignments grouped by Profile. |
|  | Permission Sets with Low Numbers of Users | Manual Report | Requires custom report on Permission Set Assignments grouped by Permission Set. |
| **Code & Customization** | Multiple Apex Triggers per Object | SFDX Scanner (PMD \- Design/Best Practice) | While not a specific rule, complexity rules (e.g., NcssMethodCount, CyclomaticComplexity 28) indirectly discourage overly complex triggers. Best practice is generally enforced via code review. |
|  | API Versions (New Code Using Old) | SFDX Scanner (PMD \- Best Practice/Error Prone) | PMD rules can potentially flag usage of deprecated features, though specific API version checks might require custom rules or manual review. Salesforce periodically enforces retirement of older API versions.8 |
|  | Hard-Coded URLs | SFDX Scanner (PMD \- Error Prone/Security) | PMD rules like ApexSuggestUsingNamedCred 28 discourage hardcoding endpoints/credentials. General hardcoded URLs might need custom rules or manual checks. |
|  | Formula Fields with JavaScript Code | Manual Review / SFDX Scanner (potentially custom rule) | Standard SFDX Scanner engines may not specifically target JavaScript within *formula fields*. Requires manual inspection or potentially custom analysis. |
|  | Migrating Workflow Rules to Flow Builder | Lightning Flow Scanner | Flow Scanner can identify Process Builders 32, implicitly encouraging migration away from older automation like Workflow Rules (which are also being retired 8). |
|  | S-Controls | Manual Review / Metadata API Query | S-Controls are legacy. Check via Setup or Metadata API query. Unlikely to be covered by modern scanners. |
|  | Apex SOQL/DML Inside Loops | SFDX Scanner (PMD \- Performance/Security) | PMD rules AvoidSOQLInLoops, AvoidDMLStatementsInLoops 28 directly address this in Apex. |
|  | Apex CRUD/FLS Violations | SFDX Scanner (PMD \- Security) | PMD rule ApexCRUDViolation 28 checks for missing access permission checks before DML/SOQL. |
|  | Apex Sharing Violations | SFDX Scanner (PMD \- Security) | PMD rule ApexSharingViolations 28 checks for missing explicit sharing declarations in classes with DML. |
|  | Apex SOQL Injection | SFDX Scanner (PMD \- Security) | PMD rule ApexSOQLInjection 28 detects use of unescaped variables in dynamic SOQL. |
| **Automation (Flows)** | Flow DML/SOQL Inside Loops | Lightning Flow Scanner | Core check of Flow Scanner to identify limit-consuming elements within loops.41 |
|  | Flow Hardcoded IDs | Lightning Flow Scanner | Flow Scanner rule HardcodedId 32 identifies hardcoded Salesforce IDs. |
|  | Flow Missing Fault Paths | Lightning Flow Scanner | Flow Scanner rule MissingFaultPath 32 checks for data elements lacking fault connectors. |
|  | Flow API Version | Lightning Flow Scanner | Flow Scanner rule APIVersion 32 checks if flows are using sufficiently recent API versions. |
|  | Flow Naming Conventions | Lightning Flow Scanner | Flow Scanner rule FlowName 32 can enforce naming conventions via regex. |
|  | Flow Missing Description | Lightning Flow Scanner | Flow Scanner rule FlowDescription 32 checks for flows lacking descriptions. |
|  | Flow System Mode Without Sharing | Lightning Flow Scanner | Flow Scanner rule SystemMode 32 flags flows configured to run in system mode without sharing, which can pose security risks. |
| **Configuration & Limits** | Active Validation Rule Limits | Manual Check / Metadata API | Requires checking limits in Setup or via Metadata API counts. Optimizer provided a count. |
|  | Active Workflow Rule Limits | Manual Check / Metadata API | Requires checking limits in Setup or via Metadata API counts. Optimizer provided a count. Workflows being retired.8 |
|  | Page Layouts per Object | Manual Check / Metadata API | Requires checking limits per object type in Setup or via Metadata API. Optimizer provided counts for objects exceeding thresholds. |
|  | Record Types per Object | Manual Check / Metadata API | Requires checking limits per object type in Setup or via Metadata API. Optimizer provided counts for objects exceeding thresholds. |
|  | My Domain | Health Check | Health Check baseline typically includes checks related to My Domain settings. |
|  | Data Storage / File Storage / Static Resource Limits | Setup / System Overview / API | Optimizer provided usage graphs.13 Alternatives require checking Setup \> Company Information, System Overview, or using APIs (e.g., Limits resource). |
| **UI & Adoption** | Lightning Components on Record Pages | Manual Review / Custom Report | Requires reviewing Lightning Record Pages or potentially building reports on component usage if tracked. |
|  | Lightning Sales/Service Console Adoption | Manual Review / Login History | Requires checking user profile assignments, app usage, or potentially Login History analysis. |
|  | Path Usage | Manual Review | Requires checking if Path is configured and potentially observing user adoption. |
|  | Notes and Attachments Related List (vs Files) | Manual Review / Custom Report | Requires reviewing page layouts and potentially reporting on ContentDocument vs Attachment usage. |
|  | Inactive Chatter Users / Incomplete Chatter Profiles | Manual Report / AppExchange | Requires custom reports on User login dates / profile completion fields, or potentially AppExchange adoption tools. |
|  | Unsupported Browsers / Out-of-Date Browsers | Login History / IT Policy | Requires analysis of Login History data (Browser field) or enforcement via corporate IT policy. |
| **Data Mgmt & Maintenance** | Duplicate Management | Manual Review / Setup | Requires checking if Duplicate Rules and Matching Rules are configured and active in Setup. |
|  | Field Usage | Manual Analysis / AppExchange / Field Trip (Unofficial) | Optimizer provided usage stats. Alternatives include manual analysis, paid AppExchange apps, or the community-provided (but potentially unsupported) Field Trip tool. |
|  | Unused Dashboards / Unused Reports | Manual Report / AppExchange | Requires custom reports based on Last Run Date / Last Viewed Date. No direct free automated tool identified. Some AppExchange cleanup tools may cover this. |
|  | Unassigned Custom Profiles / Page Layouts / Perm Sets | Manual Review / Metadata API / AppExchange | Requires checking assignments in Setup, potentially using Metadata API queries, or specialized AppExchange tools. Optimizer flagged unassigned items. |
|  | Unassigned Record Types / Roles | Manual Review / Metadata API / AppExchange | Similar to other unassigned items, requires manual checks or specialized tools. |

*Note: "Manual Review/Report" indicates that no direct, automated check was identified in the core free tools researched. Achieving this often requires navigating Setup, creating custom Salesforce reports, querying the Metadata API, or potentially using AppExchange solutions.*

### **5.2. Analysis of Coverage & Gaps**

The comparison table highlights both the strengths and limitations of the free toolset in replacing Salesforce Optimizer:

* **Strengths:**  
  * **Security Settings:** Salesforce Health Check provides robust coverage for org-level security configurations, comparing them against defined baselines.17  
  * **Code Quality & Security:** SFDX Scanner, particularly with its PMD and ESLint engines, offers deep static analysis for Apex and Lightning components, covering many security vulnerabilities (CRUD/FLS, SOQL Injection) and performance issues (SOQL/DML in loops) that Optimizer touched upon.28  
  * **Automation Health:** Lightning Flow Scanner specifically targets Flows and declarative automation, providing essential checks for performance, maintainability, and best practices in this critical area.32  
  * **Automation & Orchestration:** Megalinter enables the integration of these checks into automated CI/CD pipelines, promoting consistency and early detection.26  
* **Gaps:**  
  * **UI Adoption & Specific Feature Usage:** Optimizer included checks related to the adoption of specific UI features like Path, Lightning Consoles, or News/Twitter.6 Free static analysis tools generally do not measure user adoption. Assessing this typically requires manual checks, user surveys, or custom reporting on usage data (if available).  
  * **Comprehensive Cleanup Identification:** While scanners can find unused variables (SFDX Scanner) or potentially inactive flows (Flow Scanner), identifying unused reports, dashboards, or specific unassigned metadata components (like roles or profiles beyond simple assignment checks) often falls outside the scope of these free tools. Optimizer provided specific flags for these 6, whereas alternatives usually require manual reporting (e.g., based on last run/modified dates) or specialized (often paid) AppExchange cleanup tools.  
  * **Unified Limit Monitoring:** Optimizer offered a quick glance at key storage and resource limits, sometimes with historical graphs.13 While this data is available natively (e.g., in Setup \> Company Information or via API Limits resources), no single free tool aggregates and visualizes various limits in the same consolidated manner as Optimizer aimed to.  
  * **Contextual Recommendations:** Optimizer attempted to provide specific recommendations within its interface.4 While the alternative tools report violations, translating these findings into prioritized, actionable steps requires interpretation by the Salesforce team.

While the core technical health aspects related to security, code, and flow automation are well-addressed by the combination of Health Check, SFDX Scanner, and Flow Scanner, replicating the full breadth of Optimizer's checks, especially those related to UI adoption and certain maintenance/cleanup tasks, demands supplementary manual processes or exploring solutions beyond the free tier. Organizations must evaluate which Optimizer checks were most critical to their processes and plan accordingly to fill any gaps identified.

## **6\. Strategic Approach to Replacing Optimizer**

Simply knowing the alternative tools is insufficient; effectively replacing Salesforce Optimizer requires integrating these tools strategically into organizational processes. A multi-tool approach, embedded within existing lifecycles, is necessary to maintain org health post-Optimizer.

### **6.1. Leveraging a Multi-Tool Strategy**

Given the specialized nature of the free alternatives, a layered strategy is recommended:

* **Salesforce Health Check:** Utilize this for periodic, org-wide security posture assessments. Running it monthly, quarterly, or before major releases aligns with general security best practices.19 It provides a baseline security compliance score and highlights critical configuration risks.17  
* **SFDX Scanner (Code Analyzer):** Integrate this deeply into the development workflow. Developers should run it locally before committing code. Crucially, it should be incorporated into CI/CD pipelines to automatically scan Apex, LWC, and Aura components upon code changes, preventing problematic code from reaching production.24  
* **Lightning Flow Scanner:** Similar to SFDX Scanner, this should be part of the development and deployment process for declarative automation. Run it locally or, ideally, integrate it into CI/CD pipelines to analyze Flows before deployment.32  
* **Megalinter:** For teams utilizing CI/CD, Megalinter serves as the orchestrator. Configure it to run SFDX Scanner, Flow Scanner, and other relevant linters (e.g., secrets scanners) automatically within the pipeline, providing consolidated feedback and enforcing quality gates.26  
* **Manual Checks & Custom Reports:** Schedule regular reviews for areas not covered by automated tools, such as unused reports/dashboards (based on last run date), specific UI adoption metrics, or detailed permission set assignments.

### **6.2. Integrating Scans into Lifecycles**

To maximize value, these tools should become integral parts of standard Salesforce lifecycles:

* **Ongoing Maintenance:** Regularly scheduled Health Checks 17 and reviews of custom reports (for unused metadata, etc.) should replace the periodic Optimizer run for general org health monitoring.  
* **Development Cycle:** SFDX Scanner and Flow Scanner checks should be mandatory steps before code or flow metadata is merged or deployed. Using Megalinter in a CI/CD pipeline automates this enforcement.24 This "shift-left" approach catches issues earlier and reduces remediation costs.  
* **Release Readiness:** Before deploying significant changes, installing new managed packages, or preparing for Salesforce seasonal releases (a recommended time to run Optimizer 13), execute relevant scans (Health Check, SFDX Scanner, Flow Scanner) as part of the final validation checklist. Analyze the output to identify and mitigate potential risks introduced by the changes.

### **6.3. Prioritization Recommendations**

The output from multiple tools can be extensive. A prioritization strategy helps focus efforts:

1. **High-Severity Security Issues:** Address critical vulnerabilities flagged by Health Check (High-Risk settings 17) and SFDX Scanner (Security category rules like SOQL Injection, CRUD/FLS violations 23). These pose the most immediate risk.  
2. **Performance Bottlenecks:** Fix issues likely to impact performance and limits, such as SOQL/DML in loops identified by SFDX Scanner (PMD Performance rules 28) or Flow Scanner.41  
3. **Code Quality & Maintainability:** Gradually address warnings related to complexity, best practices, and code style from SFDX Scanner and Flow Scanner. This reduces technical debt and improves long-term maintainability.  
4. **Maintenance & Cleanup Gaps:** Periodically address findings from manual reviews or custom reports (e.g., unused components, adoption metrics) based on organizational priorities.

Effectively replacing Optimizer involves more than just substituting one tool for others; it requires evolving processes. Teams need to integrate these specialized tools into their routine maintenance, development, and release activities. This might involve establishing new manual review cadences or investing in setting up or enhancing CI/CD automation to leverage tools like Megalinter. This process adaptation is crucial for maintaining comprehensive org oversight in the absence of the single, broad overview previously offered by Optimizer.

## **7\. Conclusion**

The planned retirement of the Salesforce Optimizer app in Winter '26 1, following the earlier retirement of its PDF predecessor 3, marks a shift in how Salesforce supports org analysis. While Optimizer provided a convenient, broad overview within a single interface, its functionality can be largely replicated, and in some areas surpassed, by leveraging a combination of free, specialized tools.

No single free tool serves as a direct replacement. However, a strategic combination of:

* **Salesforce Health Check** for security configuration baseline adherence.17  
* **SFDX Scanner (Salesforce Code Analyzer)** for in-depth static analysis of custom code (Apex, LWC, Aura) covering quality, performance, and security.24  
* **Lightning Flow Scanner** for ensuring best practices and identifying issues in declarative automation (Flows).32  
* **(Optionally) Megalinter** for orchestrating these scans within automated CI/CD pipelines.26

This multi-tool approach provides substantial coverage for the core technical checks performed by Optimizer. Key areas like security settings, code vulnerabilities (CRUD/FLS, SOQL Injection), performance issues (DML/SOQL in loops), and flow health are well-addressed.

However, gaps remain, particularly concerning specific UI adoption metrics and certain maintenance/cleanup checks (like identifying unused reports/dashboards) that Optimizer flagged.6 Bridging these gaps requires supplementary manual analysis, custom reporting, or potentially exploring paid AppExchange solutions.5

Successfully navigating the Optimizer retirement requires more than just tool substitution; it necessitates a process evolution. Organizations must integrate these specialized tools into their regular maintenance routines, development workflows, and release readiness checks. Proactive adoption and strategic integration of Health Check, SFDX Scanner, and Flow Scanner, potentially automated via Megalinter, will be crucial for ensuring continued Salesforce org health, security, and performance moving forward.

#### **Sources des citations**

1. Salesforce Optimizer Retirement \- Reddit, consulté le avril 27, 2025, [https://www.reddit.com/r/salesforce/comments/1jeyx4o/salesforce\_optimizer\_retirement/](https://www.reddit.com/r/salesforce/comments/1jeyx4o/salesforce_optimizer_retirement/)  
2. Improve Your Implementation with Salesforce Optimizer, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=sf.optimizer\_introduction.htm\&type=5](https://help.salesforce.com/s/articleView?id=sf.optimizer_introduction.htm&type=5)  
3. PDF Report for Salesforce Optimizer Is Being Retired, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=release-notes.rn\_general\_optimizer.htm\&language=en\_US\&release=232\&type=5](https://help.salesforce.com/s/articleView?id=release-notes.rn_general_optimizer.htm&language=en_US&release=232&type=5)  
4. Speed Up Org Maintenance with the Optimizer App \- Salesforce Help, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=release-notes.rn\_general\_optimizer\_app.htm\&language=en\_US\&release=226\&type=5](https://help.salesforce.com/s/articleView?id=release-notes.rn_general_optimizer_app.htm&language=en_US&release=226&type=5)  
5. Is Salesforce Optimizer still down for Maintenance? No update since December 2024, consulté le avril 27, 2025, [https://www.reddit.com/r/salesforce/comments/1hwujbi/is\_salesforce\_optimizer\_still\_down\_for/](https://www.reddit.com/r/salesforce/comments/1hwujbi/is_salesforce_optimizer_still_down_for/)  
6. Features Evaluated in Salesforce Optimizer \- Salesforce Help, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=sf.optimizer\_included\_features.htm\&language=en\_US\&type=5](https://help.salesforce.com/s/articleView?id=sf.optimizer_included_features.htm&language=en_US&type=5)  
7. Guide to the Salesforce Optimizer App, consulté le avril 27, 2025, [https://www.salesforceben.com/secret-weapon-salesforce-optimizer-report/](https://www.salesforceben.com/secret-weapon-salesforce-optimizer-report/)  
8. Salesforce Product & Feature Retirements, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=000381744\&language=en\_US\&type=1](https://help.salesforce.com/s/articleView?id=000381744&language=en_US&type=1)  
9. Improve Your Implementation with Salesforce Optimizer, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=xcloud.optimizer\_introduction.htm\&language=en\_US\&type=5](https://help.salesforce.com/s/articleView?id=xcloud.optimizer_introduction.htm&language=en_US&type=5)  
10. Improve Your Implementation with Salesforce Optimizer, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=sf.optimizer\_introduction.htm\&language=en\_US\&type=5](https://help.salesforce.com/s/articleView?id=sf.optimizer_introduction.htm&language=en_US&type=5)  
11. Salesforce Health Check Guide \- Apex Hours, consulté le avril 27, 2025, [https://www.apexhours.com/salesforce-health-check/](https://www.apexhours.com/salesforce-health-check/)  
12. How To Use Salesforce Optimizer: A Complete Guide \- Tech Blog \- Emizentech, consulté le avril 27, 2025, [https://emizentech.com/blog/salesforce-optimizer.html](https://emizentech.com/blog/salesforce-optimizer.html)  
13. Run the Salesforce Optimizer App, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=sf.optimizer\_kick\_off.htm\&language=en\_US\&type=5](https://help.salesforce.com/s/articleView?id=sf.optimizer_kick_off.htm&language=en_US&type=5)  
14. Salesforce Optimizer Considerations, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=sf.optimizer\_considerations.htm\&language=en\_US\&type=5](https://help.salesforce.com/s/articleView?id=sf.optimizer_considerations.htm&language=en_US&type=5)  
15. Some Salesforce Optimizer Reports Have Been Removed, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=release-notes.rn\_general\_optimizer\_reports.htm\&language=en\_US\&release=254\&type=5](https://help.salesforce.com/s/articleView?id=release-notes.rn_general_optimizer_reports.htm&language=en_US&release=254&type=5)  
16. Security Health Check \- Salesforce Help, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=sf.security\_health\_check.htm\&language=en\_US\&type=5](https://help.salesforce.com/s/articleView?id=sf.security_health_check.htm&language=en_US&type=5)  
17. Security Health Check \- Salesforce Developers, consulté le avril 27, 2025, [https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/security\_health\_check.htm](https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/security_health_check.htm)  
18. Salesforce Health Check: Tools, Benefits and Best Practices \- Reco AI, consulté le avril 27, 2025, [https://www.reco.ai/learn/salesforce-health-check](https://www.reco.ai/learn/salesforce-health-check)  
19. Salesforce Security Health Check Essentials Guide in 2025 ☁️, consulté le avril 27, 2025, [https://www.sfapps.info/salesforce-security-health-check-process/](https://www.sfapps.info/salesforce-security-health-check-process/)  
20. How to Perform a Salesforce Health Check, consulté le avril 27, 2025, [https://www.salesforceben.com/salesforce-health-check/](https://www.salesforceben.com/salesforce-health-check/)  
21. Health Check \- Salesforce Help, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=release-notes.rn\_security\_health\_check\_changes.htm\&language=en\_US\&release=246\&type=5](https://help.salesforce.com/s/articleView?id=release-notes.rn_security_health_check_changes.htm&language=en_US&release=246&type=5)  
22. Use CLI Commands to Analyze Your Code \- Salesforce Developers, consulté le avril 27, 2025, [https://developer.salesforce.com/docs/platform/salesforce-code-analyzer/guide/setup.html](https://developer.salesforce.com/docs/platform/salesforce-code-analyzer/guide/setup.html)  
23. Scan Your Solution with Salesforce Code Analyzer | ISVforce Guide, consulté le avril 27, 2025, [https://developer.salesforce.com/docs/atlas.en-us.packagingGuide.meta/packagingGuide/security\_review\_code\_analyzer\_scan.htm](https://developer.salesforce.com/docs/atlas.en-us.packagingGuide.meta/packagingGuide/security_review_code_analyzer_scan.htm)  
24. Salesforce Code Analyzer, consulté le avril 27, 2025, [https://developer.salesforce.com/docs/platform/salesforce-code-analyzer/overview](https://developer.salesforce.com/docs/platform/salesforce-code-analyzer/overview)  
25. forcedotcom/sfdx-scanner \- GitHub, consulté le avril 27, 2025, [https://github.com/forcedotcom/sfdx-scanner](https://github.com/forcedotcom/sfdx-scanner)  
26. Quality Checks with MegaLinter (Salesforce monitoring) \- Sfdx-Hardis Documentation, consulté le avril 27, 2025, [https://sfdx-hardis.cloudity.com/salesforce-monitoring-quality-checks/](https://sfdx-hardis.cloudity.com/salesforce-monitoring-quality-checks/)  
27. scanner Commands | Salesforce CLI Command Reference, consulté le avril 27, 2025, [https://developer.salesforce.com/docs/atlas.en-us.sfdx\_cli\_reference.meta/sfdx\_cli\_reference/cli\_reference\_scanner\_commands\_unified.htm](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference_scanner_commands_unified.htm)  
28. Apex Rules | PMD Source Code Analyzer, consulté le avril 27, 2025, [https://pmd.github.io/pmd/pmd\_rules\_apex.html](https://pmd.github.io/pmd/pmd_rules_apex.html)  
29. PMD Customization | Salesforce Code Analyzer v4, consulté le avril 27, 2025, [https://developer.salesforce.com/docs/platform/salesforce-code-analyzer/guide/pmd-config.html](https://developer.salesforce.com/docs/platform/salesforce-code-analyzer/guide/pmd-config.html)  
30. Let standard PMD rules be disabled by config file/args · Issue \#246 · forcedotcom/sfdx-scanner \- GitHub, consulté le avril 27, 2025, [https://github.com/forcedotcom/sfdx-scanner/issues/246](https://github.com/forcedotcom/sfdx-scanner/issues/246)  
31. PMD and Salesforce, consulté le avril 27, 2025, [https://salesforce.stackexchange.com/questions/350643/pmd-and-salesforce](https://salesforce.stackexchange.com/questions/350643/pmd-and-salesforce)  
32. Lightning-Flow-Scanner \- GitHub, consulté le avril 27, 2025, [https://github.com/Lightning-Flow-Scanner](https://github.com/Lightning-Flow-Scanner)  
33. lightning-flow-scanner-core \- NPM, consulté le avril 27, 2025, [https://www.npmjs.com/package/lightning-flow-scanner-core](https://www.npmjs.com/package/lightning-flow-scanner-core)  
34. Lightning-Flow-Scanner/lightning-flow-scanner-core: A rule engine capable of conducting static analysis on the metadata associated with Salesforce Lightning Flows, Process Builders, and Workflows. \- GitHub, consulté le avril 27, 2025, [https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-core](https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-core)  
35. Lightning-Flow-Scanner/lightning-flow-scanner-copado \- GitHub, consulté le avril 27, 2025, [https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-copado](https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-copado)  
36. Lightning-Flow-Scanner/lightning-flow-scanner-app: A Salesforce App designed to pinpoint deviations from Industry Best Practices in Salesforce Flows, ensuring standards of business automation excellence. \- GitHub, consulté le avril 27, 2025, [https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-app](https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-app)  
37. lightning-flow-scanner configuration in MegaLinter, consulté le avril 27, 2025, [https://megalinter.io/v8/descriptors/salesforce\_lightning\_flow\_scanner/](https://megalinter.io/v8/descriptors/salesforce_lightning_flow_scanner/)  
38. Lightning-Flow-Scanner/lightning-flow-scanner-sfdx: A Salesforce CLI Plugin designed to pinpoint deviations from Industry Best Practices in Salesforce Flows, ensuring standards of business automation excellence. \- GitHub, consulté le avril 27, 2025, [https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-sfdx](https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-sfdx)  
39. Lightning Flow Scanner \- Visual Studio Marketplace, consulté le avril 27, 2025, [https://marketplace.visualstudio.com/items?itemName=ForceConfigControl.lightningflowscanner](https://marketplace.visualstudio.com/items?itemName=ForceConfigControl.lightningflowscanner)  
40. Releases · Lightning-Flow-Scanner/lightning-flow-scanner-vsce \- GitHub, consulté le avril 27, 2025, [https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-vsce/releases](https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-vsce/releases)  
41. Flow Analyzer | Salesforce AppExchange, consulté le avril 27, 2025, [https://appexchange.salesforce.com/appxListingDetail?listingId=1de21f0b-2476-4780-a6b5-1bc631ccd138](https://appexchange.salesforce.com/appxListingDetail?listingId=1de21f0b-2476-4780-a6b5-1bc631ccd138)  
42. Static Flow Analysis: How to Run Static Code Analysis for Flows \- Salesforce Ben, consulté le avril 27, 2025, [https://www.salesforceben.com/static-flow-analysis-how-to-run-static-code-analysis-for-flows/](https://www.salesforceben.com/static-flow-analysis-how-to-run-static-code-analysis-for-flows/)  
43. Lightning-Flow-Scanner/lightning-flow-scanner-vsce: A VSCode Extension designed to pinpoint deviations from Industry Best Practices in Salesforce Flows, ensuring standards of business automation excellence. \- GitHub, consulté le avril 27, 2025, [https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-vsce](https://github.com/Lightning-Flow-Scanner/lightning-flow-scanner-vsce)  
44. lightning-flow-scanner \- NPM, consulté le avril 27, 2025, [https://www.npmjs.com/package/lightning-flow-scanner](https://www.npmjs.com/package/lightning-flow-scanner)  
45. Free Salesforce Flow Analyzer Tool \- CertifyCRM, consulté le avril 27, 2025, [https://certifycrm.com/salesforce-flow-analyzer-review/](https://certifycrm.com/salesforce-flow-analyzer-review/)  
46. MegaLinter · Actions · GitHub Marketplace, consulté le avril 27, 2025, [https://github.com/marketplace/actions/megalinter](https://github.com/marketplace/actions/megalinter)  
47. MegaLinter by OX Security, consulté le avril 27, 2025, [https://megalinter.io/](https://megalinter.io/)  
48. oxsecurity/megalinter: MegaLinter analyzes 50 languages, 22 formats, 21 tooling formats, excessive copy-pastes, spelling mistakes and security issues in your repository sources with a GitHub Action, other CI tools or locally. \- GitHub, consulté le avril 27, 2025, [https://github.com/oxsecurity/megalinter](https://github.com/oxsecurity/megalinter)  
49. Install MegaLinter on GitHub Actions, consulté le avril 27, 2025, [https://megalinter.io/latest/install-github/](https://megalinter.io/latest/install-github/)  
50. salesforce flavor in MegaLinter, consulté le avril 27, 2025, [https://megalinter.io/v8/flavors/salesforce/](https://megalinter.io/v8/flavors/salesforce/)  
51. sfdx-scanner-apex \- Mega-Linter, consulté le avril 27, 2025, [https://megalinter.io/v5.0.6/descriptors/salesforce\_sfdx\_scanner\_apex/](https://megalinter.io/v5.0.6/descriptors/salesforce_sfdx_scanner_apex/)  
52. Salesforce Functions Is Being Retired, consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=release-notes.rn\_functions\_retired.htm\&language=en\_US\&type=5](https://help.salesforce.com/s/articleView?id=release-notes.rn_functions_retired.htm&language=en_US&type=5)  
53. Salesforce Platform API Versions 21.0 Through 30.0 Retirement (Release Update), consulté le avril 27, 2025, [https://help.salesforce.com/s/articleView?id=release-notes.rn\_api\_retirement\_delay\_248rn.htm\&language=en\_US\&release=248\&type=5](https://help.salesforce.com/s/articleView?id=release-notes.rn_api_retirement_delay_248rn.htm&language=en_US&release=248&type=5)  
54. PMD \- Apex Class Rules \- Quality Clouds Documentation, consulté le avril 27, 2025, [https://qualityclouds.com/documentation/qc/pmd-rules/pmd-apex-class-rules/](https://qualityclouds.com/documentation/qc/pmd-rules/pmd-apex-class-rules/)  
55. lightning-flow-scanner configuration in MegaLinter, consulté le avril 27, 2025, [https://megalinter.io/7.10.0/descriptors/salesforce\_lightning\_flow\_scanner/](https://megalinter.io/7.10.0/descriptors/salesforce_lightning_flow_scanner/)  
56. Enforce code standards with PMD \- Texeï \- Texei, consulté le avril 27, 2025, [https://texei.com/en/advices/enforce-code-standards-with-pmd/](https://texei.com/en/advices/enforce-code-standards-with-pmd/)
