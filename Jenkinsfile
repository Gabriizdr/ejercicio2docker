pipeline{
    agent any 

    environment {
        DOCKER_IMAGE = 'gabiizdr/ejercicio2docker'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Gabriizdr/ejercicio2docker.git'
            }
        }

        stage('build') {
            steps {
                script {
                    dockerImage = docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push'){
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
                        dockerImage.push('latest')
                    }
                }
            }
            post {
                success {
                    bat 'echo  "Docker image pushed successfully!"'
                }
                failure {
                    bat 'echo "Failed to push Docker image."'
                }
            }
        }
    }
}