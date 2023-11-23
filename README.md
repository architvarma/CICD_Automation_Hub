# MuleSoft-DevOps
# Jenkins Pipelines Configuration Readme

## 1. Conditional Check Pipeline

### 1.1 Directory Structure

```
Dev-Pipeline/
| - Condition Check/
|   | - Jenkinsfile (Mulesoft-Conditional_Check Pipeline)
|   | - repository_name.txt
|   | - processed_commits.txt
| - Jenkinsfile (Mulesoft-Dev Pipeline)
```

### 1.2 Condition-Check Pipeline Steps

The `Condition-Check` pipeline reads the repository name from `repository_name.txt`, fetches commit IDs with specific commit messages from all branches (excluding `main` and `develop`), and stores them in `processed_commits.txt`. The `Mulesoft-Dev` pipeline is automatically triggered based on specified conditions.

Adjustments have been made to consider existing processed commits, and the condition `if (commitMessage.contains("Build:") && !processedCommits.contains(commitId))` ensures only new commit IDs trigger the `Mulesoft-Dev` pipeline.

## 2. Mulesoft-Dev Pipeline

### 2.1 Pipeline Overview

The `Mulesoft-Dev` pipeline has parameters `GitUrl` and `GitBranch` and consists of five stages:

1. **Version Validation and Checkout:**
   - Clones the repository using the specified Git URL and branch.
   - Checks the version in the `pom.xml` file.
   - Compares the version with the latest version in the JFrog repository.
   - Increases the version if necessary.
   - Updates the Pom Version for later stages.

2. **Check Code Conflict:**
   - Attempts to merge the specified branch with the develop branch.
   - Fails if a code conflict is detected.

3. **Build:**
   - Runs Maven commands to build the JAR file.
   - Extracts configuration parameters from `properties.jenkins`.

4. **Deploy Development:**
   - Runs Maven commands to deploy the JAR file to Anypoint Platform.
   - Considers Anypoint deployment configuration from `pom.xml`.

5. **Publish to JFrog Artifactory:**
   - Uploads the deployed JAR file to JFrog Artifactory using a curl command.
   - Extracts artifact name for the Prod environment from `properties.jenkins`.

Customize the curl command, file paths, property names, and JFrog Artifactory details based on your project structure and environment.

## 3. Mulesoft QA/Pre-Prod Pipeline

### 3.1 Pipeline Overview

The `Mulesoft QA/Pre-Prod` pipeline has parameters `GIT_URL`, `GIT_BRANCH`, `Environment`, `Artifact_Package`, and `Mule_Env`. It consists of five stages:

1. **Version Validation Checkout:**
   - Clones the repository based on the provided GIT_URL and GIT_BRANCH.
   - Fetches the latest tags and compares them with the `Artifact_Package` parameter.

2. **Check Code Conflict:**
   - Attempts to merge the specified branch with the develop branch.
   - Fails if a code conflict is detected.

3. **Download Artifact:**
   - Downloads the JAR file based on the version provided in `Artifact_Package`.
   - Extracts group ID from `pom.xml` and reads artifact name from `properties.jenkins`.

4. **Promotion to QA/PreProd:**
   - Checks Nexus package existence.
   - Obtains user approval for promotion.
   - Allows deployment target selection for QA environment.

5. **Deployment to QA/PreProd:**
   - Runs Maven commands to deploy the JAR file to Anypoint Platform.
   - Extracts configuration parameters from `properties.jenkins` and considers `pom.xml` Anypoint deployment configuration.

## 4. Mulesoft Production Pipeline

### 4.1 Pipeline Overview

The `Mulesoft Production` pipeline has parameters `GIT_URL`, `GIT_BRANCH`, `Environment`, `Artifact_Package`, and `Deployment_Target`. It consists of six stages:

1. **Version Validation Checkout:**
   - Clones the repository based on the provided GIT_URL and GIT_BRANCH.
   - Fetches the latest tags and compares them with the `Artifact_Package` parameter.

2. **Check Code Conflict:**
   - Attempts to merge the specified branch with the develop branch.
   - Fails if a code conflict is detected.

3. **Download Artifact:**
   - Downloads the JAR file based on the version provided in `Artifact_Package`.
   - Extracts group ID from `pom.xml` and reads artifact name from `properties.jenkins`.

4. **Promotion to Production:**
   - Obtains user approval.
   - Deploys the application into the CloudHub/Hybrid Production Environment.

5. **Deployment to Production:**
   - Runs Maven commands to deploy the JAR file to Anypoint Platform.
   - Extracts configuration parameters from `properties.jenkins` and considers `pom.xml` Anypoint deployment configuration.

6. **Promotion to Post Production:**
   - Obtains user approval for post-production actions.
   - Performs a merge into the Develop and Main branches.
   - Handles abort scenarios, asking for reasons and setting build status accordingly.
