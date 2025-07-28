pipeline{
    agent any 

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
                bat 'echo "Building the project..."'
            }
        }
    }
}