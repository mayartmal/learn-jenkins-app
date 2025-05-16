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
                // cleanWs()
                sh '''
                     echo "Building ... ... ..."
                     ls -la
                     node --version
                     npm --version
                     npm ci
                     npm run build
                     echo "Building almost complete ... ... ..."
                     ls -la
                '''
            }
        }

        stage('Test') {
            steps{
                sh '''
                    echo "Test stage in progress ... ... ..."
                    test -f build/index.html
                    npm test
                '''
            }
            
        }



    }    
    post {
        success{
            //archiveArtifacts artifacts
            echo 'Hello from post->success'
        }
    }
}
