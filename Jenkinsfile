pipeline{
    agent any ##configuración del agente general con any

    trigger {
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
                sh 'echo "Building the project..."'
            }
        }
    }
}