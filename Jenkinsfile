pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Check or Install Node.js') {
            steps {
                script {
                    def nodeInstalled = sh(script: 'command -v node', returnStatus: true) == 0
                    if (!nodeInstalled) {
                        echo "Node.js not found. Installing latest version..."
                        sh '''
                            apt-get update
                            apt-get install -y curl ca-certificates
                            curl -fsSL https://deb.nodesource.com/setup_current.x | bash -
                            apt-get install -y nodejs
                        '''
                    } else {
                        echo "Node.js is already installed:"
                        sh 'node -v && npm -v'
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install project dependencies
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests using npm
                    sh 'npm test'
                }
            }
        }
    } // <-- closes stages
} // <-- closes pipeline