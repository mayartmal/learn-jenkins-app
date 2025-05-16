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
    )    
    post {
        success{
            //archiveArtifacts artifacts
            echo 'phello from post->success'
        }
    }
}
