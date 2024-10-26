# Flask Application with Jenkins CI/CD Pipeline
This project demonstrates the setup of a Flask application with a Jenkins CI/CD pipeline that automatically builds, tests, and deploys the application from a GitHub repository. This README outlines how to set up the Flask app, configure the CI/CD pipeline, and includes step-by-step documentation of the process.

# Table of Contents
1. Project Overview
2. Technologies Used
3. Installation and Setup
     . Local Environment Setup
     . Fork the Flask Repository
4. Jenkins Setup
     . Installing Jenkins
     . Creating a Jenkinsfile
     . Configuring Pipeline Triggers
     . Testing the Pipeline
5. Conclusion

# Project Overview
This project demonstrates how to automate the deployment process of a Flask application using Jenkins. The application is hosted on GitHub, and Jenkins is used to build, test, and deploy the application automatically upon code changes.

# Technologies Used
. Flask: A micro web framework written in Python.
. Git: Version control system to manage and track changes in the codebase.
. GitHub: Remote repository for hosting the project.
. Jenkins: Open-source automation server used to automate parts of the software development process.
. Python: Programming language used to build the Flask application.

# Installation and Setup
## Local Environment Setup

1. Install Python:
  
       sudo apt update
       sudo apt install python3 python3-pip
2. Install Flask:

       pip3 install flask

   ![Screenshot 2024-10-26 151204](https://github.com/user-attachments/assets/7bc1e4f3-db4b-46e7-96a0-5b9423a871aa)

4. Clone the Repository: 

       git clone https://github.com/AbhayShukla1907/flask..git
       cd flask.
   ![Screenshot 2024-10-26 151311](https://github.com/user-attachments/assets/e4198911-2a17-4468-bbba-d56fa69e1251)


## Fork the Flask Repository
Go to GitHub and fork the following sample Flask app repository:
     . Flask Sample Repo

# Jenkins Setup
## Installing Jenkins
To install Jenkins on Ubuntu
1. Update package index:

       sudo apt update
2. Install Java (Jenkins requires Java to run):

       sudo apt install openjdk-11-jdk
![Screenshot 2024-10-25 180712](https://github.com/user-attachments/assets/04f2a4c5-7e5c-42c1-b77e-88f62e327f80)

4. Add Jenkins repository and install Jenkins:

       curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
           /usr/share/keyrings/jenkins-keyring.asc > /dev/null
       sudo sh -c 'echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
           https://pkg.jenkins.io/debian binary/ > \
          /etc/apt/sources.list.d/jenkins.list'
       sudo apt update
       sudo apt install jenkins

5. Start Jenkins:

       sudo systemctl start jenkins
Visit http://localhost:8080 and complete the setup by using the initial admin password:
         
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword

## Creating a Jenkinsfile
The Jenkinsfile defines the CI/CD pipeline stages for Flask application. Here's a basic example:

           pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Install dependencies
                    sh 'pip3 install -r requirements.txt'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests with pytest
                    sh 'pytest tests/'
                }
            }
        }

        stage('Deploy') {
            when {
                expression {
                    return currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    // For now, simply echoing a success message.
                    // You can replace this with actual deployment steps.
                    echo "Deploying the Flask application!"
                }
            }
        }
    }
    
    post {
        always {
            // Clean workspace after build
            cleanWs()
        }
        success {
            // Notify on successful build
            mail to: 'abhay06072002@gmail.com
                 subject: "SUCCESS: Jenkins Build #${env.BUILD_NUMBER}",
                 body: "The build has been successfully completed!"
        }
        failure {
            // Notify on failed build
            mail to: 'abhay06072002@gmail.com',
                 subject: "FAILURE: Jenkins Build #${env.BUILD_NUMBER}",
                 body: "The build failed."
        }
    }
}

This Jenkinsfile performs the following tasks:

. Clone repository: It pulls the latest code from the GitHub repository.
. Install dependencies: Installs required dependencies listed in "requirements.txt."
. Run Tests: Runs unit tests.
. Deploy: Placeholder for deployment steps.

## Configuring Pipeline Triggers
To set up GitHub Webhooks and configure Pipeline Triggers:
![Screenshot 2024-10-26 213007](https://github.com/user-attachments/assets/ad05e0ef-73bd-4aa1-882f-35da89b9faa6)

1. Install the "GitHub Integration" plugin in Jenkins.
2. Configure Webhook in GitHub:
. Go to your repository's settings.
. Under Webhooks, click Add webhook.
. Set the Payload URL to http://192.168.43.62:8080/github-webhook/.
. Set Content Type to application/json.
. Choose Just the push event for triggering the webhook.
![Screenshot 2024-10-26 213351](https://github.com/user-attachments/assets/895e03d0-f280-4be8-8935-cbaf370e4b8d)

4. Configure Jenkins Job:
. Create a Pipeline job in Jenkins.
. Under Build Triggers, select GitHub hook trigger for GITScm polling.
![Screenshot 2024-10-26 213523](https://github.com/user-attachments/assets/8fbbf4aa-66e0-4a58-bd34-45e93ac8a65d)

##  Testing the Pipeline
Once Jenkins pipeline is configured:
1. Make changes to Flask repository.
2. Push the changes to GitHub:

       git add .
       git commit -m "Made some changes"
       git push origin main

3. Jenkins will automatically detect the changes via the webhook and trigger the build.
   ![Screenshot 2024-10-25 223913](https://github.com/user-attachments/assets/3a97c6bf-d86d-433f-a4b2-8a9a4bef8455)



# Conclusion
By following these steps, we successfully set up a CI/CD pipeline for Flask application using Jenkins. Each time code is pushed to the GitHub repository, Jenkins will automatically run the pipeline, test the code, and deploy it.




















