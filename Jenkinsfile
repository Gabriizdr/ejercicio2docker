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
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
    }
}