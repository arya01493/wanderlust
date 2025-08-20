pipeline {
    agent any

    environment {
        # You can override these with Jenkins Credentials later
        PORT = "4000"
        NODE_ENV = "development"
        JWT_SECRET = "supersecretkey"
        MONGO_URI = "mongodb://localhost:27017/wanderlust"
        GOOGLE_CLIENT_ID = "dummy-client-id"
        GOOGLE_CLIENT_SECRET = "dummy-client-secret"
        GOOGLE_CALLBACK_URL = "http://localhost:4000/auth/google/callback"
        SESSION_SECRET = "supersecretkey"
        EMAIL_HOST = "smtp.gmail.com"
        EMAIL_PORT = "587"
        EMAIL_USER = "your-email@gmail.com"
        EMAIL_PASS = "your-email-password"
        REDIS_HOST = "127.0.0.1"
        REDIS_PORT = "6379"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/pl-bujji05/wanderlust.git'
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Create .env File') {
            steps {
                dir('backend') {
                    sh '''
                    cat > .env <<EOL
PORT=${PORT}
NODE_ENV=${NODE_ENV}
JWT_SECRET=${JWT_SECRET}
MONGO_URI=${MONGO_URI}
GOOGLE_CLIENT_ID=${GOOGLE_CLIENT_ID}
GOOGLE_CLIENT_SECRET=${GOOGLE_CLIENT_SECRET}
GOOGLE_CALLBACK_URL=${GOOGLE_CALLBACK_URL}
SESSION_SECRET=${SESSION_SECRET}
EMAIL_HOST=${EMAIL_HOST}
EMAIL_PORT=${EMAIL_PORT}
EMAIL_USER=${EMAIL_USER}
EMAIL_PASS=${EMAIL_PASS}
REDIS_HOST=${REDIS_HOST}
REDIS_PORT=${REDIS_PORT}
EOL
                    '''
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Start Backend') {
            steps {
                dir('backend') {
                    sh 'nohup npm start &'
                }
            }
        }

        stage('Start Frontend') {
            steps {
                dir('frontend') {
                    sh 'nohup npm start &'
                }
            }
        }
    }
}
