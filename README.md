# Project Name

A full-stack web application deployed using Docker, with CI/CD automation, MongoDB database, and Nginx reverse proxy. This project demonstrates end-to-end setup, deployment, and infrastructure management.

---

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Repository Setup](#repository-setup)  
3. [Frontend Setup and Deployment](#frontend-setup-and-deployment)  
4. [Backend Setup and Deployment](#backend-setup-and-deployment)  
5. [Database Setup](#database-setup)  
6. [Docker Compose Deployment](#docker-compose-deployment)  
7. [CI/CD Pipeline Setup](#cicd-pipeline-setup)  
8. [Nginx Reverse Proxy Setup](#nginx-reverse-proxy-setup)  
9. [Screenshots](#screenshots)  
    

---

## Project Overview

This project includes:

- **Frontend:** Angular
- **Backend:** Node.js/Express API server.  
- **Database:** MongoDB  
- **Containerization:** Docker for frontend, backend, and database  
- **Deployment:** Docker Compose on an Ubuntu virtual machine  
- **CI/CD:** Jenkins for automated build, push, and deployment  
- **Reverse Proxy:** Nginx to expose services on port 80  

---

## Repository Setup

1. Create a new repository on GitHub for the project.  
2. Clone the repository locally.  
3. Copy your frontend and backend code into the repository folder.  
4. Commit and push the initial code to GitHub.

---

## Frontend Setup and Deployment

1. Create a Dockerfile for the frontend application in the frontend folder.  
2. Build a Docker image for the frontend using your Docker build command.  
3. Push the Docker image to Docker Hub so it can be deployed on any machine.

---

## Backend Setup and Deployment

1. Create a Dockerfile for the backend application in the backend folder.  
2. Build a Docker image for the backend using your Docker build command.  
3. Push the backend Docker image to Docker Hub.

---

## Database Setup
 
1. **Use MongoDB Docker Image:** Include MongoDB as a service in your Docker Compose setup, with persistent data using Docker volumes.

---

## Docker Compose Deployment

1. Create a Docker Compose configuration in the project root to define the frontend, backend, and database services.  
2. Specify the ports and volume mapping for MongoDB to ensure persistent storage.  
3. On the Ubuntu VM, run Docker Compose to start all the services in detached mode.  
4. Verify that the frontend is accessible via the specified port and the backend API is running correctly.

---

## CI/CD Pipeline Setup

1. Use Jenkins to automate the build and deployment process.  
2. Trigger the pipeline on every push to the main branch.  
3. Steps include:  
   - Checkout repository  
   - Log in to Docker Hub  
   - Build frontend and backend Docker images  
   - Push images to Docker Hub  
   - Deploy to application using Docker Compose 
4. Ensure all secrets like Docker Hub credentials stored in the jenkins credentials

---

## Nginx Reverse Proxy Setup

1. Install Nginx using nginx image from DockHub.  
2. Configure Nginx to forward requests to the frontend and backend applications:  
   - Frontend served on the root path `/`  
   - Backend API served on `/api`    
3. After configuration, the entire application should be accessible through the VM’s IP address on port 80.

---

