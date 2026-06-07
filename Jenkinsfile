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


                  npm ci
                  npm test
                  npm run build
                '''
            }
        }
        stage("Test"){
          agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
          }
          steps{
            sh '''
            npm test
            if [ -f build/index.html ]; then
              echo "EXISTS"
            else
              echo "DOES NOT EXIST"
            fi
            '''
          }
          
          
        }
        stage('Serve'){
          agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
          steps{
            sh '''
             npm install -g serve
             serve -s build
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