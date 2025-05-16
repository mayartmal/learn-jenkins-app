pipeline {
    agent any

    stages {
        stage('w/o docker') {
            steps {
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
