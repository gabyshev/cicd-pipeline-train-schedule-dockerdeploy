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
        stage('build docker image') {
            steps {
                script {
                    docker.build("train-app:${env.BUILD_ID}")
                }
            }
        }
        stage('push docker image') {
            steps {
                docker.push("train-app:${env.BUILD_ID}")
            }
        }
    }
}
