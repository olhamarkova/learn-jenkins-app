pipeline {
    agent any

    environment {
      NETLIFY_SITE_ID = '73cf9036-6177-49e3-a0fe-aa0dee16a169'
      NETLIFY_AUTH_TOKEN = credentials('netlify_token')
    }

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

        stage('Deploy'){
          agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
            }
          }
            steps {
              sh '''
                npm install netlify-cli
                node_modules/.bin/netlify --version
                echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                node_modules/.bin/netlify status
                node_modules/.bin/netlify deploy --dir=build --prod 
              '''
            }
        }
    }
}