pipeline {
    agent any

    environment {
        // You can override these with Jenkins Credentials later
        DOCKER_IMAGE = "wanderlust-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/arya01493/wanderlust.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }

        stage('Docker Run') {
            steps {
                sh "docker run -d -p 3000:3000 --name wanderlust_container ${DOCKER_IMAGE}:latest"
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker ps -aq --filter "name=wanderlust_container" | xargs -r docker rm -f || true'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
