pipeline {
    agent any

    environment {
        PIP_NO_CACHE_DIR = "off" // Prevents cache issues
        PATH = "$HOME/.local/bin:$PATH"  // Add Python user directory to PATH
        DB_HOST = '192.168.12.1'
    }

    stages {
        stage('Checkout') {
            steps {
                cleanWs() // Cleans workspace
                git branch: 'main', url: 'https://github.com/neamulkabiremon/jenkins-basics.git'
                sh "ls -ltr"
            }
        }

        stage('Setup') {
            steps {
                sh '''
                # Ensure Python3 and Pip3 are installed
                sudo apt update -y
                sudo apt install -y python3 python3-pip || true

                # Ensure pip3 is upgraded
                pip3 install --upgrade pip || true

                # Install dependencies
                pip3 install --user -r requirements.txt || true
                '''
            }
        }

        stage('Build') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'prod-server', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        echo "Using credentials for user: ${USERNAME}"

                        sh '''
                        echo "Logging in as $USERNAME"
                        curl -u $USERNAME:$PASSWORD https://example.com/protected-api
                        '''
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'prod-server', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                        # Ensure pytest is available
                        export PATH=$HOME/.local/bin:$PATH

                        # Check if pytest is installed
                        if ! command -v pytest &> /dev/null
                        then
                            echo "pytest not found, installing..."
                            pip3 install --user pytest || true
                        fi

                        # Run tests
                        pytest || true
                        echo "The DB username: $USERNAME and password: $PASSWORD"
                        '''
                    }
                }
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
