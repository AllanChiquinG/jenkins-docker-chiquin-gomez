pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkins-docker-chiquin"
        CONTAINER_NAME = "web-chiquin"
        APP_PORT = "8081"
    }

    stages {

        stage('Clonación') {
            steps {
                echo "Repositorio obtenido desde GitHub (checkout automático de Jenkins)."
                checkout scm
            }
        }

        stage('Verificación') {
            steps {
                script {
                    if (!fileExists('index.html')) {
                        error("No se encontró index.html")
                    }
                    if (!fileExists('Dockerfile')) {
                        error("No se encontró Dockerfile")
                    }
                }
                echo "Verificación exitosa: index.html y Dockerfile existen."
            }
        }

        stage('Construcción') {
            steps {
                bat "docker build -t %IMAGE_NAME%:%BUILD_NUMBER% ."
            }
        }

        stage('Despliegue') {
            steps {
                script {
                    bat "docker rm -f %CONTAINER_NAME% || exit 0"
                }
                bat "docker run -d --name %CONTAINER_NAME% -p %APP_PORT%:80 %IMAGE_NAME%:%BUILD_NUMBER%"
            }
        }

        stage('Confirmación') {
            steps {
                echo "Despliegue exitoso. Aplicación disponible en http://localhost:${APP_PORT}"
            }
        }
    }

    post {
        success {
            echo " Pipeline ejecutado exitosamente. Imagen: ${IMAGE_NAME}:${BUILD_NUMBER}"
        }
        failure {
            echo " El pipeline falló. Revisa la salida de consola para más detalles."
        }
    }
}