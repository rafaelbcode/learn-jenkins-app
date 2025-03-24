pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = 'a3d5cee2-ec05-4d83-9626-2a9bdca4e173'
    }

    stages {
        stage('Build') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test'){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''test -f build/index.html
                      npm test
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo 'Deploying to netlify'
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
