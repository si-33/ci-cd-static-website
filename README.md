# CI/CD Pipeline for Full-Stack Website

---

## Overview

This repository demonstrates a robust Continuous Integration/Continuous Delivery (CI/CD) pipeline designed to automate the deployment of **full-stack applications**. Leveraging **GitHub Webhooks**, **Jenkins**, and **Docker**, this setup ensures that every code push by a developer triggers an automated build, image creation, and deployment, making the process fast, reliable, and consistent.

---

## Features

* **Automated Builds:** Automatically compiles and packages your full-stack application for deployment upon code changes.
* **Dockerized Deployment:** Packages the entire application (frontend, backend, dependencies) into a Docker image, ensuring a consistent environment across development and production.
* **Continuous Integration:** Integrates code changes frequently, allowing for early detection of issues.
* **Continuous Delivery:** Automates the delivery of the full-stack application to a production-ready server.
* **Efficient Workflow:** Streamlines the path from code commit to live application.

---

## Technologies Used

* **GitHub:** Version control and source code hosting.
* **GitHub Webhooks:** Triggers for events (e.g., `push`) in the GitHub repository.
* **Jenkins:** Automation server for orchestrating the CI/CD pipeline.
* **Docker:** Containerization platform for building and packaging the application.
* **Docker Hub:** Cloud-based registry service for storing and distributing Docker images.
* **Apache HTTP Server:** (Potentially) serves the frontend or acts as a reverse proxy for the backend on the server machine.

---

## CI/CD Workflow

This pipeline automates the entire process from code commit to live deployment:

1.  **Developer Pushes Code:** A developer commits and pushes code changes to the GitHub repository.
2.  **GitHub Webhook Trigger:** GitHub, configured with a **Webhook**, detects the `push` event and sends a notification to the configured Jenkins server.
3.  **Jenkins Build Trigger:** Jenkins receives the webhook notification and automatically triggers a new build job for the full-stack application project.
4.  **Docker Image Creation (Jenkins):**
    * Jenkins fetches the latest code from the GitHub repository.
    * It then builds a **Docker image** containing the entire full-stack application, including its dependencies, backend server, and frontend assets.
5.  **Docker Image Push to Docker Hub:** Once the Docker image is successfully built, Jenkins pushes this new image to **Docker Hub**. This centralizes image storage and makes it accessible for deployment.
6.  **Image Download on Server Machine:** The production server (or staging server) is configured to pull the latest Docker image from Docker Hub.
7.  **Application Deployment:**
    * The Docker image is downloaded to the server machine.
    * A new Docker container is run from this image. This container encapsulates the entire full-stack application.
    * If the application is designed to serve its own frontend or requires an Apache reverse proxy for the backend, Apache can be configured accordingly. For applications where the Docker container itself serves the content (e.g., Node.js app serving its own frontend), the `/var/www/html` step may be replaced by direct container exposure or proxying.
    * In cases where static frontend assets are still moved to Apache, the process would be: The application's build artifacts (e.g., compiled frontend code) are extracted from the Docker image or built separately and then moved to `/var/www/html` if Apache is serving the frontend, while the backend runs as another service or within the same container.
    * The full-stack application is now live and accessible.

```mermaid
graph TD
    A[Developer Pushes Code to GitHub] --> B(GitHub Webhook)
    B --> C(Jenkins Server)
    C -- "Fetches Code & Builds Docker Image for Full-Stack App" --> D(Docker Daemon on Jenkins)
    D --> E(Docker Image)
    E -- "Pushes Image" --> F(Docker Hub)
    F -- "Pulls Latest Image" --> G(Server Machine)
    G -- "Runs Docker Container (Full-Stack App)" --> H(Application Process/Container)
    H -- "(Optional: serves frontend via Apache)" --> I(Apache HTTP Server)
    I --> J[Live Full-Stack Application]
