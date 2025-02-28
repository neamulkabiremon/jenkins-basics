pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                cleanWs() // Ensures a clean workspace before cloning
                git branch: 'main', url: 'https://github.com/neamulkabiremon/jenkins-basics.git'
                sh "ls -ltr"
            }
        }

        stage('Setup') {
            steps {
                sh "pip install -r requirements.txt"
            }
        }
        
        stage('Test') {
            steps {
                sh "pytest"
            }
        }

        stage('Deployment') {
            input {
                message "Do you want to proceed further?"
                ok "Yes"
            }
            steps {
                echo "Running Deployment"
            }
        }
    }
}
