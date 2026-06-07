pipeline {
    agent any

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
                  npm test
                  npm run build
                '''
            }
        }
        stage("Test"){
          steps{
            sh '''
            if [ -f build/index.html ]; then
              echo "EXISTS"
            else
              echo "DOES NOT EXIST"
            fi
            '''
          }
          
          
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'build/**'
            cleanWs()
        }
        failure {
            cleanWs()
        }
    }
}