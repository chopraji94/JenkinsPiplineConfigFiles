# Jenkins Pipeline Config Files

This repository contains sample Jenkins Pipeline configuration files (Groovy/Jenkinsfile) for different automation workflows. These pipelines are designed to be reused and adapted across various projects.

## 🚀 Contents

### 1. Groovy-based Declarative & Scripted Pipelines
- `Jenkinsfile-nodejs-declarative.groovy`: Declarative Pipeline for a Node.js app — installs dependencies, runs the app, and performs a health check.
- `Jenkinsfile-python-flask-pipeline.groovy`: Pipeline for a Python Flask app — sets up a virtual environment, runs tests, and starts the application.

*(Adjust file names to match your actual files.)*

## 🛠️ Usage

### Setup
1. Copy the relevant Jenkinsfile into the root of your project repository.
2. In Jenkins, create a **Pipeline project**, select **Pipeline script from SCM**, and configure:
   - **SCM**: Git (or GitHub)
   - **Repository URL**: Your project repo URL
   - **Script Path**: The Jenkinsfile name (e.g., `Jenkinsfile-nodejs-declarative.groovy`)

### Run Pipeline
- Trigger manually via **Build Now**, or configure **webhooks** for automated CI/CD.
- Monitor **console output** for logs of build, install, run, and cleanup stages.

### Customize
- Modify ports, timeouts, or dependency commands in their respective stages.
- Extend stages for testing, coverage, containerization, or deployment.

## 📚 Best Practices
- Name Jenkinsfiles clearly to reflect stack and purpose.
- Keep pipelines DRY and modular — consider using shared libraries for common logic.
- Store environment variables and secrets securely using Jenkins credentials or config providers.
- Include cleanup steps in `post` sections to maintain workspace hygiene.

## ✅ Example: Node.js Declarative Pipeline

```groovy
pipeline {
  agent any

  stages {
    stage('Checkout Code') {
      steps { checkout scm }
    }
    stage('Install Dependencies') {
      steps { sh 'npm install' }
    }
    stage('Run App') {
      steps {
        sh 'pkill -f "node app.js" || true'
        sh 'nohup node app.js > app.log 2>&1 &'
        sleep 60
        sh 'curl http://localhost:3001 || echo "App not responding!"'
      }
    }
  }

  post {
    always { echo 'Cleaning up...' }
  }
}
