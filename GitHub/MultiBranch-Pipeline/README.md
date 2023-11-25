# MultiBranch Pipeline

This repository hosts a Jenkins pipeline script designed for automating GitHub repository creation and configuration. The pipeline encompasses the following key stage:
**Stage1, Stage2 are comman as of [Repository-Branch-Creation Pipeline](https://github.com/architvarma/CICD_Automation_Hub/blob/main/GitHub/Repository-Branch-Creation/Jenkinsfile)**
## 3. Create GitHub Branches
   - Checks out the `main` branch and initializes the repository.
   - Copies template files to the workspace if specified.
   - Configures global Git user information.
   - Creates and pushes the `dev` branch.
   - Creates and pushes multiple feature branches based on the provided branch names.
   - Displays the Git log with a graph representation of the branches.

## Usage Instructions
1. Configure Jenkins with the necessary credentials (e.g., GitHub token).
2. Customize pipeline parameters (`params.GIT_REPO` and `params.GIT_BRANCH`) according to your project.
3. Run the pipeline to automate GitHub repository setup, webhook creation, and branch initialization.

Feel free to adapt and extend the pipeline to meet specific project requirements. Ensure proper credential management and validation for security considerations. For more details on GitHub API usage, refer to the [GitHub REST API documentation](https://docs.github.com/rest).
