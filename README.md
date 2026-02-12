📌 Project Overview
Built an end-to-end CI/CD pipeline for a Django-based Notes Application using Jenkins Pipeline-as-Code with Groovy Shared Libraries. Automated Docker image builds, tagged and pushed artifacts to DockerHub, and deployed multi-container setup using Docker Compose — achieving full automation from source code commit to production deployment.

🏗️ Architecture
Developer (Code Push)
        ↓
    GitHub Repo
        ↓
  Jenkins Pipeline
   ┌────────────┐
   │ Code Clone │
   └─────┬──────┘
         ↓
   ┌────────────┐
   │Docker Build│
   └─────┬──────┘
         ↓
   ┌─────────────┐
   │Push DockerHub│
   └─────┬────────┘
         ↓
   ┌────────────┐
   │   Deploy   │
   └─────┬──────┘
         ↓
  Docker Compose
  ┌──────────────┐
  │ django_cont  │ → Port 8000
  │ nginx_cont   │ → Port 80
  └──────────────┘

🛠️ Tech Stack
ToolPurposeDjangoWeb Application FrameworkJenkinsCI/CD Automation ServerGroovy Shared LibrariesReusable Pipeline FunctionsDockerContainerizationDockerHubDocker Image RegistryDocker ComposeMulti-Container OrchestrationNginxReverse Proxy / Web Server

✨ Key Features

✅ Pipeline-as-Code using Groovy Jenkinsfile
✅ Jenkins Shared Libraries for reusable pipeline functions
✅ Automated Docker image builds on every commit
✅ DockerHub integration for image storage and versioning
✅ Multi-container deployment using Docker Compose
✅ End-to-end automation from code commit to deployment


📁 Project Structure
django-notes-app/
├── mynotes/               # Django App
├── notesapp/              # Django Project Settings
├── templates/             # HTML Templates
├── Dockerfile             # Docker Image Configuration
├── docker-compose.yml     # Multi-Container Setup
├── Jenkinsfile            # CI/CD Pipeline Script
├── requirements.txt       # Python Dependencies
└── manage.py              # Django Management Script

🔧 Jenkins Shared Library Structure
Shared/
└── vars/
    ├── clone.groovy         # Git Clone Function
    ├── dockerbuild.groovy   # Docker Build Function
    ├── dockerpush.groovy    # DockerHub Push Function
    └── deploy.groovy        # Docker Compose Deploy Function

🚀 Jenkins Pipeline
groovy@Library('Shared') _
pipeline {
    agent any
    stages {
        stage("Code Clone") {
            steps {
                clone("https://github.com/username/django-notes-app.git", "main")
            }
        }
        stage("Docker Build") {
            steps {
                dockerbuild("notes-app", "latest")
            }
        }
        stage("Push to DockerHub") {
            steps {
                dockerpush("dockerHubCreds", "notes-app", "latest")
            }
        }
        stage("Deploy") {
            steps {
                deploy()
            }
        }
    }
}

⚙️ Setup & Installation
Prerequisites

Jenkins installed and running
Docker & Docker Compose installed
DockerHub account
GitHub account

Step 1: Clone the Repository
bashgit clone https://github.com/your-username/django-notes-app.git
cd django-notes-app
Step 2: Add DockerHub Credentials in Jenkins
Manage Jenkins → Credentials → Global → Add Credentials
Kind     : Username with password
ID       : dockerHubCreds
Step 3: Add Shared Library in Jenkins
Manage Jenkins → System → Global Pipeline Libraries
Name            : Shared
Default Version : main
Repo URL        : https://github.com/your-username/Shared
Step 4: Create Pipeline in Jenkins
New Item → Pipeline → Paste Jenkinsfile script → Save → Build Now

🐳 Docker Compose Services
yamlservices:
  django:
    image: notes-app:latest
    ports:
      - "8000:8000"

  nginx:
    image: nginx:latest
    ports:
      - "80:80"

🌐 Access the Application
http://localhost       → Notes App (via Nginx)
http://localhost:8000  → Django App (direct)
http://localhost:8080  → Jenkins Dashboard
