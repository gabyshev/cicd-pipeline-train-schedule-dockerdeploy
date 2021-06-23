pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('build docker image and push') {
            steps {
                docker.withRegistry('https://docker.mycorp.com/', 'docker_credentials') {
                    docker.build("train-app:${env.BUILD_ID}").push()
                }
            }
        }
    }
}
