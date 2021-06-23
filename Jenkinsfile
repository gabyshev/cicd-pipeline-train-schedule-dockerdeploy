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
                script {
                    docker.withRegistry('https://hub.docker.com/', 'docker_credentials') {
                        docker.build("train-app:${env.BUILD_ID}").push()
                    }
                }
            }
        }
    }
}
