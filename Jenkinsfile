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
            mail to: 'your_email@example.com',
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
