pipeline {
    agent any

    environment {
        NODE_VERSION = '18' // Adjust to match your project's Node.js version
    }

    tools {
        nodejs "${NODE_VERSION}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }

           stage('Install Node.js Manually') {
            steps {
                sh '''
                curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION}.x | sudo -E bash -
                sudo apt-get install -y nodejs
                node -v
                npm -v
                '''
            }
        }


        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Start Application') {
            steps {
                echo 'Starting the application...'
                // If needed in background (e.g., for integration tests), use `nohup` or `&`
                sh 'npm run start &'
                sleep(time: 5, unit: 'SECONDS') // Give app time to start
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'pkill -f "node"' // Or use process manager like `pm2` if needed
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
