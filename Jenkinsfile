pipeline {
    agent any
    environment{
        NETLIFY_SITE_ID = '76fd5b27-cddf-4ade-ac99-c7a6a0b583b5'
        NETLIFY_AUTH_TOKEN = credentials('NETLIFY-TOKEN')
    }
    stages {
        stage('Build') {
            agent{
                docker{
                    image  'node:18-alpine'
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
            agent{
                docker{
                    image  'node:18-alpine'
                    reuseNode true
                }
            }
            
            steps{
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('Deploy') {
            agent{
                docker{
                    image  'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli -g
                    netlify --version
                    echo "Deploying to production"
                    netlify status
                '''
            }
        }
    }
    post {
        always{
            junit 'test-results/junit.xml'
        }
    }
}
