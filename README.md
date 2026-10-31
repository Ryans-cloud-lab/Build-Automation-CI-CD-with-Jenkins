# Build Automation & CICD with Jenkins

**Project:** CI/CD pipeline for a Java Maven application with Jenkins (build, containerize, publish, version automation)

**Technologies used:** Jenkins, Docker, DigitalOcean, Linux, Git, GitHub, Java, Maven, Groovy

**Scope:**

- Install and run Jenkins on a DigitalOcean Droplet (Jenkins as a Docker container)
- Configure build tools (Maven) and Docker on the Jenkins server
- Create Jenkins credentials for GitHub and Docker Hub
- Implement CI with Pipeline as Code (`Jenkinsfile`): build JAR, build Docker image, push to a private Docker Hub repository
- Explore Freestyle, Pipeline, and Multibranch Pipeline job types
- Integrate a Jenkins Shared Library for reusable build steps
- Trigger builds automatically via GitHub webhook
- Dynamically increment application version, tag images with version + build number, and commit version updates back to Git

---

## Description
This repository contains a Java Maven application (java-maven-app) with a Jenkinsfile that versions, builds, containerises and publishes it along with a Jenkins Shared Library [jenkins-shared-library](jenkins-shared-library) with reusable build steps.

The setup mirrors a typical company workflow: a dedicated Jenkins server, credentials for external systems kept in Jenkins, pipeline as code in Git, images published to a registry, and builds triggered automatically by code changes.

The sample application and starting pipeline come from the bootcamp. I set up the Jenkins server, adapted the pipeline and shared library to my own accounts and repository layout, and documented the problems I solved along the way.

## Prerequisites

Before reproducing this project on a new Jenkins server or cloning for local development:

- **DigitalOcean Droplet** (or other Linux host) for Jenkins
- **Docker** and **Docker Compose** on the Jenkins host (Jenkins runs as a container; the agent needs Docker for image build/push)
- **GitHub** repository with **webhook** access to your Jenkins URL
- **Docker Hub** account (or private registry) for image push
- **Java 17** and **Maven 3.9+** on the agent, aligned with [java-maven-app/pom.xml](./java-maven-app/pom.xml)
- Jenkins **Global Tool Configuration** for Maven (tool name must match the Jenkinsfile on your branch, e.g. `Maven` on `jenkins-jobs`)
- Jenkins **credentials** (Global scope): `docker-hub-repo`, `github-pat-devops-08` (and `github-credentials` on some branches)—see [Jenkins configuration](#jenkins-configuration-checklist) below

**Example application:** [java-maven-app](./java-maven-app/)
---

## What was implemented

### Infrastructure

- **Jenkins** running as a Docker container (jenkins/jenkins:lts) on an Ubuntu droplet on DigitalOcean, with Jenkins home on a named volume so jobs, -plugins and credentials survive container restarts
- **Restart policy** unless-stopped, so Jenkins comes back on its own after a plugin restart or a server reboot
- **Docker** made available to the Jenkins container through the host's Docker socket, so pipelines can build and push images
- **Maven** configured as a Jenkins-managed tool; Node.js and npm installed inside the container
- ###### Port 8080 open in the DigitalOcean firewall for the Jenkins UI and GitHub webhooks

### CI pipeline (end state on `jenkins-jobs`)

The active pipeline in [java-maven-app/Jenkinsfile](./java-maven-app/Jenkinsfile) on branch **`jenkins-jobs`** runs these stages:

| Stage | Purpose |
|-------|---------|
| **increment version** | Bump the Maven **patch** version in `pom.xml` using `build-helper:parse-version` and `versions:set`; derive `IMAGE_NAME` as `{version}-{BUILD_NUMBER}` |
| **build app** | `mvn clean package` — compile, test, and produce the executable JAR |
| **build image** | `docker build`, `docker login`, `docker push` to **Docker Hub** (`ronkaiser86/demo-app:${IMAGE_NAME}`) |
| **deploy** | Placeholder stage (deployment to a remote host is extended in later modules) |
| **commit version update** | Commit the updated `pom.xml` and push back to Git so the repo records the new version |

### Jenkins job types and learning artifacts

On branch **`main`**, multiple files document the course progression without replacing the live pipeline on `jenkins-jobs`:

| File | Illustrates |
|------|-------------|
| [Jenkinsfile](./java-maven-app/Jenkinsfile) | Full CI pipeline with versioning (evolved alongside `jenkins-jobs`) |
| [Jenkinsfile-pipeline](./java-maven-app/Jenkinsfile-pipeline) | Declarative Pipeline loading [script.groovy](./java-maven-app/script.groovy) for build / image / deploy |
| [Jenkinsfile-syntax](./java-maven-app/Jenkinsfile-syntax) | Parameters, `when` conditions, and user **input** for environment selection |
| Embedded `jenkins-shared-library/` (on `main` only) | Local copy of shared library structure while learning |

Branch **`jenkins-shared-lib`** wires the external shared library into the Pipeline (`buildJar()`, `dockerLogin()`, `dockerPush()`, etc.).

Branch **`feature/payment`** supports **Multibranch Pipeline** demos (separate branch for branch-specific behavior).
