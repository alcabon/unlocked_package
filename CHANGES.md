Okay, acting as a fellow Salesforce Admin, here's a summary of significant upcoming feature retirements that generally require notable maintenance effort, along with tools to help assess the impact on your specific org. Keep in mind that "major" depends on your org's usage, but these are common ones causing significant migration work.

Key Upcoming Retirements Requiring Major Maintenance (as of late April 2025):

# Workflow Rules & Process Builder:

## Retirement: End of support is scheduled for December 31, 2025. While they might function past this date, there will be no support, bug fixes, or enhancements, and Salesforce strongly advises migration.
Maintenance Required: This is a major undertaking for most orgs. All automation built using these tools needs to be migrated to Salesforce Flow. This involves analysis, rebuilding logic in Flow, testing, and deployment.
Why it's major: These tools often underpin critical business processes. Migration requires understanding Flow capabilities and potentially redesigning automation logic.

# Chat (Live Agent) & Messaging (Legacy):

## Retirement: Chat (Live Agent) is scheduled for retirement on February 14, 2026.
Maintenance Required: You need to migrate to the modern Messaging for In-App and Web. This involves setting up new channels, configuring routing (likely with Omni-Channel and Flow), updating website snippets, training agents, and potentially adjusting reporting.
Why it's major: It impacts core customer service channels, requires learning a new setup (Messaging Session objects, Omni-Channel Flows), and often involves web development changes.

# Salesforce for Outlook (Legacy Client):

## Retirement: Rescheduled to December 1, 2027.
Maintenance Required: Migrate users to the Outlook Integration (often paired with Einstein Activity Capture or Salesforce Inbox features). This requires rolling out the new add-in, configuring sync settings (EAC requires careful planning around data storage and direction), managing user permissions, and significant change management/user training.
Why it's major: Affects daily sales user workflows directly within Outlook. EAC introduces different sync behaviors compared to the old client, which needs clear communication.

# Platform API Versions 21.0 through 30.0:

## Retirement: Scheduled for Summer '25 (around June 2025).
Maintenance Required: Any custom code (Apex), integrations (middleware, third-party apps), or installed packages making calls using these legacy API versions must be identified and updated to use a supported, more recent API version (v31.0+).
Why it's major: Failure to update can break critical integrations and custom functionality. Identifying all usage points across potentially many systems and codebases can be time-consuming.

# Maintenance Plan Frequency Fields (Field Service):

## Retirement: Migration required before the Winter '26 release (fields become unavailable). Salesforce recommended migrating by October 1, 2025.
Maintenance Required: Migrate existing Maintenance Plans using the legacy Frequency and Frequency Type fields to use Maintenance Work Rules. This involves analyzing existing plans and configuring new work rules to replicate the scheduling logic.
Why it's major: Essential for orgs using Field Service preventative maintenance. Failure to migrate means work orders will stop generating from these plans.

# Tools to Evaluate Changes and Impact:

## Salesforce Setup > Release Updates:

What it is: This is your primary native tool. Salesforce uses this section to announce upcoming changes and retirements that require action.
How it helps: For many retirements (like the Maintenance Plan Frequency Fields or JDK Locale Formats mentioned in the past), Salesforce provides specific analyzers accessible via Release Updates. These tools can scan your org to identify where the retiring feature is being used (e.g., which specific Workflow Rules, Maintenance Plans, etc.). Activate these updates in a sandbox first to assess the impact.

## Salesforce Optimizer:

What it is: A native tool that generates a report on your org's health, configuration, and areas for improvement.
How it helps: While not solely focused on retirements, the Optimizer report can flag usage of older features, limits approaching, and areas needing maintenance, which might overlap with retired features (e.g., highlighting profiles that could be converted to permission sets, or identifying automation complexity).

## Salesforce Dependency API:

What it is: An API that allows you to query the relationships between metadata components in your org.
How it helps: You (or developers/tools) can use this to understand what other components (Apex classes, Flows, fields, etc.) might be affected if you change or remove a specific component related to a retired feature.

# Third-Party / AppExchange Tools (Metadata Intelligence / Org Analysis):

What they are: Several tools specialize in analyzing org complexity, metadata dependencies, and managing technical debt.
How they help: These often provide more advanced visualization and impact analysis than native tools, making it easier to understand complex relationships, find hardcoded references, and manage large migration projects (like WFR/PB to Flow). Examples include:
- Elements.cloud: Strong focus on metadata dependency analysis and visualization.
- Salto (incl. Free Tier): Offers impact analysis and full-text search across metadata.
- Metazoa Snapshot: Offers free reports like Org Dictionary, Field Usage, etc.
- Strongpoint.io: Built on-platform, focuses on impact analysis and change management.
Note: Evaluate these based on your org's complexity and budget.

# Manual Code/Metadata Search:

What it is: Using tools like VS Code with Salesforce Extensions, Salesforce CLI, or even the Developer Console search.
How it helps: Allows you to search your codebase (Apex, LWC, Aura, Visualforce) and potentially metadata XML files for references to specific fields, objects, or API versions related to retired features. Essential for API version retirement analysis.

# Trailblazer Community Groups & Official Documentation:

What they are: Online forums and Salesforce Help pages.
How they help: Often, specific Trailblazer Community groups are created for major retirements (like Flow migration). These are great places to ask questions and find shared strategies. Always refer to the official Salesforce Product & Feature Retirements page on Salesforce Help for the latest dates and details.

# Recommendations:

Start Early: Don't wait until the deadline. Migrations like WFR/PB to Flow can take significant time.
Use Sandboxes: Thoroughly test all migrations and changes in a sandbox environment before deploying to production.
Prioritize: Focus on the retirements impacting your most critical business processes first.
Document: Keep track of your analysis, migration plan, and testing results.
Regularly check the Release Updates section in Setup and the official Salesforce retirement documentation to stay informed. Good luck!
