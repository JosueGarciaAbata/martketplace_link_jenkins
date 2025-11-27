pipeline {
    agent any

    environment {

        // Imagen Docker del backend
        DOCKER_IMAGE_TAG = "dockerfile-marketplace"
        DOCKER_IMAGE_FILE = "Dockerfile-marketplace"

        // Contenedor donde correrá la aplicación
        CONTAINER_NAME = "marketplace_backend"

        // Red Docker donde está la base de datos
        DOCKER_NETWORK = "mplink_net"

        // SPRING BOOT
        SPRING_PROFILES_ACTIVE = "prod"
        SERVER_PORT = "8090"
        FRONTEND_URL = "*"
        SUSPENDED_TIME_DAYS = "3"

        // BASE DE DATOS PRINCIPAL
        DB_HOST = "marketplace_db"
        DB_PORT = "5432"
        DB_NAME = "marketplace_db"
        DB_USER = "postgres"
        DB_PASSWORD = "admin"

        // CORREO
        MAIL_HOST = "smtp.gmail.com"
        MAIL_PORT = "587"
        MAIL_USERNAME = "deividjosue52@gmail.com"
        MAIL_PASSWORD = "brofrrjvuyvhqcfe"

        // MODERADOR
        MODERATOR_DEFAULT_PASSWORD = "SecretPasswordM123"

        // AZURE STORAGE
        AZURE_STORAGE_ENABLED = "false"
        AZURE_STORAGE_CONNECTION_STRING = "placeholder"
        AZURE_STORAGE_CONTAINER_NAME = "imagenes"
    }

    stages {

        stage('Build WAR') {
            steps {
                echo "Compilando backend..."
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Construyendo imagen Docker del backend..."
                sh "docker build -t ${DOCKER_IMAGE_TAG} -f ${DOCKER_IMAGE_FILE} ."
            }
        }

        stage('Deploy Backend Container') {
             steps {
                        echo "Eliminando contenedor anterior si existe..."
                        sh "docker rm -f ${CONTAINER_NAME} || true"

                        echo "Iniciando nuevo contenedor del backend..."
                        sh """
                        docker run -d \
                          --name ${CONTAINER_NAME} \
                          --network ${DOCKER_NETWORK} \
                          -p ${SERVER_PORT}:${SERVER_PORT} \
                          -e SPRING_PROFILES_ACTIVE=${SPRING_PROFILES_ACTIVE} \
                          -e SERVER_PORT=${SERVER_PORT} \
                          -e FRONTEND_URL=${FRONTEND_URL} \
                          -e SUSPENDED_TIME_DAYS=${SUSPENDED_TIME_DAYS} \
                          -e DB_HOST=${DB_HOST} \
                          -e DB_PORT=${DB_PORT} \
                          -e DB_NAME=${DB_NAME} \
                          -e DB_USER=${DB_USER} \
                          -e DB_PASSWORD=${DB_PASSWORD} \
                          -e MAIL_HOST=${MAIL_HOST} \
                          -e MAIL_PORT=${MAIL_PORT} \
                          -e MAIL_USERNAME=${MAIL_USERNAME} \
                          -e MAIL_PASSWORD=${MAIL_PASSWORD} \
                          -e MODERATOR_DEFAULT_PASSWORD=${MODERATOR_DEFAULT_PASSWORD} \
                          -e AZURE_STORAGE_ENABLED=${AZURE_STORAGE_ENABLED} \
                          -e AZURE_STORAGE_CONNECTION_STRING="${AZURE_STORAGE_CONNECTION_STRING}" \
                          -e AZURE_STORAGE_CONTAINER_NAME=${AZURE_STORAGE_CONTAINER_NAME} \
                          ${DOCKER_IMAGE_TAG}
                        """
                    }
                }
    }
}
