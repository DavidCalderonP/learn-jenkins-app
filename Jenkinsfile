pipeline {
    agent any
    // Cuidado con las variables de entorno porque pueden colisionar con las variables del propio framework
    /*
    */
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    rm -rf build/**
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        }
        stage('E2E'){
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.64.1-noble'
                    reuseNode true
                    args ''
                }
            }
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build
                    npx playwright test
                '''
            }
        }
    }
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
