pipeline {
    agent any

        stages {
            stage('Clean Workspace') {
              steps {
                  deleteDir()
              }
            }

        stage('Build') {
          agent{
            docker{
              image 'node:18-alpine'
              reuseNode true
            }
          }
            steps {
                sh '''
                  if [ -f index.html ]; then
                      echo "True"
                  fi
                  ls -la
                  node --version
                  npm --version
                  
                  npm ci
                  npm run build
                  npm test
                  ls -la
                '''
            }
        }
        
    }
    post{
      success{
        archiveArtifacts artifacts: "**"
        
        cleanWs();
      }
      failure{
        cleanWs();
      }
    }
}
