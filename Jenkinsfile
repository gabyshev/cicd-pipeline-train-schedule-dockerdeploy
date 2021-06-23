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
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("gabyshev/train-app")
                }
            }
        }
        stage('push docker image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry("https://registry.hub.docker.com", 'docker_credentials') {
                        app.push("${env.BUILD_ID}")
                    }
                }

            }
        }
        stage('deploy on prod') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'deployer', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        sh "sshpass -p '$PASSWORD' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_id \"docker pull gabyshev/train-app:${env.BUILD_ID}\""
                        try {
                            sh "sshpass -p '$PASSWORD' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_id \"docker stop train-app\""
                            sh "sshpass -p '$PASSWORD' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_id \"docker rm train-app\""
                        } catch (err) {
                            echo "err: $err"
                        }
                        sh "sshpass -p '$PASSWORD' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_id \"docker run --restart-always -p 8080:8080 --name train-app -d gabyshev/train-app:${env.BUILD_ID}\""
                    }
                }
            }
        }
    }
}
