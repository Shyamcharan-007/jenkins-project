# Jenkins CI/CD Pipeline for Java Web Application

## Project Overview

This project demonstrates how to build and automate a Continuous Integration and Continuous Delivery (CI/CD) pipeline using Jenkins.

A Java web application is built using Maven, analyzed using SonarQube, packaged into a Docker image, and pushed to Docker Hub. The pipeline also updates the Kubernetes deployment manifest with the latest Docker image version and pushes the updated file to GitHub.

The project demonstrates how different DevOps tools work together to automate application build, code quality analysis, containerization, and deployment configuration management.

## Architecture Diagram

The following architecture illustrates the integration of different DevOps tools used in this project.

![Jenkins CI/CD System Architecture](images/architecture.png)

### Architecture Explanation

- **Developer:** Makes changes to the application source code.
- **GitHub:** Stores the source code and project configuration files in the `testing` branch.
- **Jenkins:** Automates the CI/CD pipeline.
- **SonarQube:** Performs source code quality analysis.
- **Maven:** Compiles and packages the Java application into a WAR file.
- **Docker:** Builds a container image for the application.
- **Docker Hub:** Stores the generated Docker images.
- **Kubernetes:** Deployment manifests are maintained and updated. Actual cluster deployment is planned as a future enhancement.
- **Jenkins Credentials:** Securely manages authentication tokens and passwords.

## CI/CD Pipeline Flow Diagram

The following flowchart explains the sequence of stages executed by Jenkins during a pipeline build.

![Jenkins CI/CD Pipeline Flow](images/cicd-pipeline.png)

### Pipeline Flow Explanation

1. **Start:** Developer initiates a code update.
2. **GitHub:** Updated source code is pushed to the repository.
3. **Checkout:** Jenkins retrieves the latest source code from the `testing` branch.
4. **SonarQube Analysis:** Application code is analyzed for quality issues.
5. **Maven Build:** Maven compiles and packages the application.
6. **WAR Artifact:** The generated WAR file is stored in the `target` directory.
7. **Docker Image:** Docker creates a container image using the Dockerfile.
8. **Docker Hub:** Jenkins authenticates and pushes the generated image.
9. **Update Deployment File:** Jenkins updates the image tag in `deploymentfiles/deployment.yml`.
10. **GitHub Push:** The updated deployment manifest is committed and pushed to the `testing` branch.
11. **Pipeline Completed:** All configured stages finish successfully.

## Technologies Used

| Technology      | Purpose                                          |
| --------------- | ------------------------------------------------ |
| Git             | Version control                                  |
| GitHub          | Source code repository                           |
| Jenkins         | CI/CD automation                                 |
| Maven           | Java application build                           |
| SonarQube       | Code quality analysis                            |
| Docker          | Application containerization                     |
| Docker Hub      | Container image registry                         |
| Kubernetes      | Container orchestration and deployment manifests |
| Linux / AWS EC2 | Jenkins execution environment                    |

## Project Workflow

### 1. Source Code Management – GitHub

- Application source code is maintained in GitHub.
- Jenkins checks out the code from the `testing` branch.
- GitHub stores the application code, Jenkins pipeline, Dockerfile, and Kubernetes configuration files.

### 2. Continuous Integration – Jenkins

Jenkins automates the pipeline using a Jenkinsfile.

The pipeline contains the following stages:

| Stage                  | Description                                                                                 |
| ---------------------- | ------------------------------------------------------------------------------------------- |
| Checkout               | Retrieves source code from GitHub                                                           |
| SonarQube Analysis     | Checks application code quality                                                             |
| Build Artifact         | Builds the Java web application using Maven                                                 |
| Docker Image           | Creates a Docker image                                                                      |
| Push to Docker Hub     | Uploads the image to Docker Hub                                                             |
| Update Deployment File | Updates the image version in the Kubernetes deployment YAML and pushes the change to GitHub |

### 3. Code Quality Analysis – SonarQube

SonarQube analyzes the application source code to identify code quality issues and potential bugs.

Jenkins securely accesses the SonarQube token through Jenkins Credentials.

### 4. Build Application – Maven

Maven compiles and packages the Java web application into a WAR file.

Command:
```
mvn clean package
```

The generated WAR artifact is stored in the `target` directory.

### 5. Containerization – Docker

Docker packages the application into a container image using a multi-stage Docker build.

Example:
```
docker build -t shyamcharan/jenkins-project:1 .
```

### 6. Push Image – Docker Hub

Jenkins authenticates with Docker Hub using securely stored credentials and pushes the generated image.

Example:
```
docker push shyamcharan/jenkins-project:1
```

### 7. Kubernetes Deployment Configuration

Jenkins updates the Docker image tag inside:
```
deploymentfiles/deployment.yml
```

The updated deployment manifest is committed and pushed to the GitHub `testing` branch.

**Note:** This pipeline currently updates the Kubernetes deployment configuration. Applying the manifest to a Kubernetes cluster is a separate deployment step that can be added later.

## Repository Structure
```
jenkins-project/
│
├── deploymentfiles/
│   ├── deployment.yml
│   └── service.yml
│
├── src/
│   └── main/
│       └── webapp/
│           ├── WEB-INF/
│           │   └── web.xml
│           └── index.jsp
│
├── Dockerfile
├── Jenkinsfile
├── pom.xml
└── README.md
```

## Jenkins Credentials Configuration

Sensitive credentials are stored in Jenkins Credentials instead of being hardcoded in the Jenkinsfile.

| Credential ID     | Purpose                                      |
| ----------------- | -------------------------------------------- |
| `sonar-token`     | SonarQube authentication                     |
| `dockerhub-creds` | Docker Hub authentication                    |
| `githubtoken`     | GitHub authentication using token-based push |

Credentials are accessed through Jenkins credential bindings.

**Security:** Never upload passwords, access tokens, or secret keys to GitHub.

## Prerequisites

Before running the pipeline, configure:

- Jenkins server and required plugins
- Java and Maven
- Git and GitHub repository access
- SonarQube server and authentication token
- Docker installed and accessible to the Jenkins agent
- Docker Hub account and repository
- Jenkins credentials with the correct IDs
- Kubernetes manifests

## How to Run the Project

1. Clone the GitHub repository.
2. Configure the required Jenkins tools and credentials.
3. Create a Jenkins Pipeline job.
4. Select **Pipeline script from SCM**.
5. Select **Git** as the SCM.
6. Enter the repository URL:

   `https://github.com/Shyamcharan-007/jenkins-project.git`
7. Set the branch to `testing`.
8. Set the script path to `Jenkinsfile`.
9. Save the job and start a build.
10. Monitor the pipeline stages in Jenkins.

## Expected Outcome

After a successful pipeline execution:

- Source code is checked out from GitHub.
- SonarQube analysis is completed.
- Maven generates the WAR artifact.
- Docker creates the application image.
- Docker Hub receives the image.
- Kubernetes deployment YAML is updated with the new image tag.
- The updated deployment file is pushed to GitHub.

## Future Enhancements

- Deploy the application automatically to a Kubernetes cluster.
- Add automated testing stages.
- Add email notifications for pipeline results.
- Integrate Kubernetes monitoring.
- Implement rollback strategies for failed deployments.

## Author

**Shyam R**

GitHub: [Shyamcharan-007](https://github.com/Shyamcharan-007)

Project Repository: [jenkins-project](https://github.com/Shyamcharan-007/jenkins-project)

---

*This project is developed for learning and demonstrating practical DevOps CI/CD concepts and tool integration.*
