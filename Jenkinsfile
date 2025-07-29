pipeline{ //son importantes en los flujos y es la palabra reservada pipeline, serie de pasos a ejecutar para que la app funcione
    agent any //tipo de agente especifico, tipo any, para una configuración general

    environment {
        DOCKER_IMAGE = 'gabiizdr/ejercicio2docker' //variable de entorno, para que no tengamos que escribir el nombre de la imagen cada vez que la queramos usar, se usa para que sea más fácil de leer y mantener
    }

    triggers { //para que se ejecute de forma automática cada cierto tiempo
        githubPush() //se ejecuta cuando se hace un push a GitHub metodo predefinod dentro del plugin, el plugin nos da la opción de tener este metodo accesible dentro de nuestro jenkinsfile, si no tuvieramos el prugis, mo podríamos tener este metodo
    }

    stages { //son agrupaciones de ejecuciones o cosas a ejeutar como el stage de bildeo, de entrega, stage de pushe, de pruebas etc 
        stage('checkout') { //es un stage de checkout, para que jenkins sepa que tiene que hacer un checkout del repositorio
            steps {
                git branch: 'main', url: 'https://github.com/Gabriizdr/ejercicio2docker.git'
            }
        }

        stage('build') {
            steps {
                script { //se usa script para poder usar el lenguaje de programación dentro del jenkinsfile, para poder usar las variables y demás
                    dockerImage = docker.build(DOCKER_IMAGE) //es una variable que esta dentro del pipeline y el resultado se guarda en la variable dockerImage
                }
            }
        }

        stage('Push'){
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') { //con esta línea nos conectamos al registro de Docker Hub, el segundo parámetro es el ID de las credenciales que tenemos en Jenkins, urilizamos el metodo de docker .withRegistry y las credenciales (identificador especifico que colocamos)para conectarnos al registro de Docker Hub, utiliza el registry que va a apuntar y el segundo son las crednciales pero es el idc
                        dockerImage.push('latest') //se hace el docker build y docker push concatenado donde nostraemos la iamgen y la pusheamos, y recibe en los parentesis el nombre del tag que le queremos poner a la imagen, en este caso latest, que es el tag por defecto de docker, si no se especifica un tag, se usa latest
                    }
                }
            }
            post { //post es una sección que se ejecuta después de que se haya completado el stage, ya sea con éxito o con error
                success {
                    bat 'echo  "Docker image pushed successfully!"'
                }
                failure {
                    script{
                        def logLines = currentBuild.rawBuild.getLog(100) //obtenemos las últimas 100 líneas del log de la construcción actual
                        def logText = logLines.join('\n') //unimos las líneas en un solo texto
                        mail to: 'gaby94dr@gmail.com',
                         from: 'gaby94dr@gmail.com',
                         subject: "Failed to build docker image ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                         body: "Failed to build docker image ${env.JOB_NAME} #${env.BUILD_NUMBER} ${env.BUILD_URL} \n\n ${logText}" //enviamos un correo electrónico con el asunto, cuerpo y demás, enviamos el log del error
                    }
                    
                }
            }
        }
    }
}