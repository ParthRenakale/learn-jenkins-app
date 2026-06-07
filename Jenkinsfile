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

                  echo "=== package.json ==="
                  grep -n "typescript" package.json || true
                  grep -n "@testing-library/dom" package.json || true

                  echo "=== package-lock.json ==="
                  grep -n '"typescript"' package-lock.json | head || true
                  grep -n '"@testing-library/dom"' package-lock.json | head || true
                  grep -n '"picocolors"' package-lock.json | head || true
                  npm ci
                  npm test
                  npm run build
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