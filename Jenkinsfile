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
                    echo "Application build will be added here"
                '''
            }
        }

    }

    post {
        success {
            echo '===== CI PIPELINE SUCCESS ====='
        }

        failure {
            echo '===== CI PIPELINE FAILED ====='
        }
    }
}
