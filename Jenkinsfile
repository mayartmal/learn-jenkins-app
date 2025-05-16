pipeline {
    agent any

    stages {
        stage('w/o docker') {
            steps {
                sh '''
                     echo "Without Docker"
                     #npm --version
                     ls -la
                     touch container-no.txt
                '''
            }
        }
        stage('w/ docker'){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
                
            }
            steps{
                sh '''
                    echo "With Docker"
                    npm --version
                    ls -la
                    touch container-yes.txt
                '''
            }
        }
    }
    post {
        success{
            //archiveArtifacts artifacts
            echo 'postStage'
        }
    }
}
