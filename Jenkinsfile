pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                cleanWs() // Clean workspace before cloning
                git branch: 'main', url: 'https://github.com/neamulkabiremon/jenkins-basics.git'
                sh "ls -ltr"
            }
        }

        stage('Setup') {
            steps {
                sh '''
                # Install Python and Pip (if missing)
                sudo apt update -y
                sudo apt install -y python3 python3-pip
                pip3 install --upgrade pip
                
                # Install dependencies
                pip3 install -r requirements.txt
                '''
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
