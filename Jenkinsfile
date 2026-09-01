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
stage('Docker Run Test') {
    steps {
        sh '''
            echo "===== Docker Run Test ====="

            docker rm -f mern-server-ci-test 2>/dev/null || true

            docker run -d \
                --name mern-server-ci-test \
                -p 3001:3000 \
                --network mern-ecommerce_app-network \
                -e PORT=3000 \
                -e MONGO_URI=mongodb://mongo:27017/mern_ecommerce \
                -e JWT_SECRET=ci-test-secret \
                -e CLIENT_URL=http://localhost:8080 \
                -e BASE_API_URL=api \
                mern-ecommerce-server:${BUILD_NUMBER}

            sleep 10

            docker ps --filter "name=mern-server-ci-test"

            docker logs mern-server-ci-test

            docker inspect mern-server-ci-test \
                --format '{{.State.Status}}'

            docker rm -f mern-server-ci-test
        '''
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
