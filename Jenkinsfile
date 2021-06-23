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
                    app = docker.build("gabyshev/train-app")
                }
            }
        }
        stage('push docker image') {
            steps {
                script {
                    docker.withRegistry("https://registry.hub.docker.com", 'docker_credentials') {
                        app.push("${env.BUILD_ID}")
                    }
                }

            }
        }
    }
}
