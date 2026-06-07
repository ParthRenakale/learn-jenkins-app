pipeline {
    agent any

    stages {
        stage('Build') {
          agent{
            docker{
              image 'node:18-alpine'
              reuseNode true
            }
          }
            steps {
                sh '''
                  if [ -f build/index.html ]; then
                      echo "True"
                  fi
                  ls -la
                  node --version
                  npm --version
                  npm install
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
        archiveArtifacts artifacts: "build/**"
        
        cleanWs();
      }
      failure{
        cleanWs();
      }
    }
}
