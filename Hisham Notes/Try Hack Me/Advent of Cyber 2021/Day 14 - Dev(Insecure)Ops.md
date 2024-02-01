# Day 14 - Networking - Dev(Insecure)Ops

## What is CI/CD?
- CI: Continuous Integration is the process in which software source code is kept in a central repository (such as GitHub). All changes are stored in this central repository to avoid ending up with different versions of the same code.  

- CD: Continuous Delivery is the following (sometimes integral) step of the continuous integration model where code is automatically deployed to the test, pre-production, or production environments. CD is sometimes used as an acronym for "Continuous Deployment".

- DevOps teams typically use software such as Jenkins, GitLab, Bamboo, AWS CodePipeline, etc., to automate CI/CD steps summarized above.

## Risks

-   Access security: The increasing number of integration points can make access management difficult. Allowing too much access can also open a path for malicious activity.
-   Permissions: Components are connected with each other and perform their tasks with user accounts. Similar to access security, user permissions should be checked.
-   Keys and secrets: Many integrations are done using keys (API keys, ID keys, etc.) or secrets. These should be secured. 
-   User security: User accounts are another successful attack vector often used by cybercriminals. Any user who has access to the source code repository could include a malicious component in the codebase and could be included in the deployed application
-   Default configuration: Some platforms are known to have default credentials and vulnerabilities. If the default credentials are not changed, and in use within the CI/CD process, this could result in the complete compromise of the infrastructure.