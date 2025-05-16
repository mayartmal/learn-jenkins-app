pipeline {
    agent any

    stages {
        stage('Stage') {
            steps {
                cleanWs()
                sh '''
                     echo "Hello frome stages->stage->step"
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
