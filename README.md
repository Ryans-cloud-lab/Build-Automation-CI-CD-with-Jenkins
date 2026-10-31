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
This repository contains a Java Maven application (java-maven-app) with a Jenkinsfile that versions, builds, containerises and publishes it along with a Jenkins Shared Library [jenkins-shared-library](https://github.com/Ryans-cloud-lab/jenkins-shared-library) with reusable build steps.

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
