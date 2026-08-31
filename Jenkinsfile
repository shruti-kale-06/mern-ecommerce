pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Environment Check') {
            steps {
                sh '''
                    echo "===== Environment ====="
                    pwd
                    git --version
                    node --version
                    npm --version
                    docker --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    echo "===== Installing Dependencies ====="
                    npm install
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    echo "===== Build Stage ====="
                    cd client
                    npm run build
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    echo "===== Docker Build ====="
                    docker build -t mern-ecommerce-server:${BUILD_NUMBER} ./server
                '''
            }
        }
    }

    post {
        success {
            echo "===== CI PIPELINE SUCCESS ====="
            archiveArtifacts artifacts: 'client/dist/**', fingerprint: true
        }

        failure {
            echo "===== CI PIPELINE FAILED ====="
        }
    }
}
