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
