# Repository-Branch-Creation #

This repository contains a Jenkins pipeline script for automating various GitHub operations. The pipeline includes the creation of a new repository, setting up a webhook, and creating branches. The script leverages GitHub API calls to achieve seamless integration and automation.

## Pipeline Stages

### 1. **Create GitHub Repository**
   - Checks if the repository already exists.
   - If not, creates a new private repository with an initial commit.

### 2. **Create a Webhook**
   - Sets up a webhook for the repository to trigger on push and pull request events.
   - Configures the webhook to notify a specified URL.

### 3. **Create GitHub Branch**
   - Checks out the `main` branch.
   - Copies files from the `files` directory to the repository workspace.
   - Commits an initial empty commit to `main`.
   - Creates and pushes a new `dev` branch.
   - Creates and pushes a new branch based on the provided branch name (`gitBranch`).

## Usage
1. Configure the necessary credentials in Jenkins for GitHub operations.
2. Customize the pipeline variables (`params.GIT_REPO`, `params.GIT_BRANCH`, and others) according to your project requirements.
3. Run the pipeline in Jenkins to automate GitHub repository setup and branch creation.

Feel free to adapt and extend the pipeline to suit your specific needs and workflows. For more information on GitHub API usage, refer to the [GitHub REST API documentation](https://docs.github.com/rest).
