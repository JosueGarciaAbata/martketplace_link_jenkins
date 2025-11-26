pipeline {
    agent any

    environment {
        // Nombre de la imagen del backend
        DOCKER_IMAGE = "dockerfile-marketplace"

        // Nombre del contenedor donde correrá tu app
        CONTAINER_NAME = "marketplace_backend"

        // Red Docker donde está tu base
        DOCKER_NETWORK = "mplink_net"

        // Variables de entorno del backend (Spring Boot)
        SERVER_PORT = "8090"
        FRONTEND_URL = "http://localhost:5174"

        SPRING_PROFILES_ACTIVE = "prod"

        DB_HOST = "mplink_marketplace_db"
        DB_PORT = "5432"
        DB_NAME = "marketplace_db"
        DB_USER = "marketplace"
        DB_PASSWORD = "admin"

        MAIL_HOST = "smtp.gmail.com"
        MAIL_PORT = "587"
        MAIL_USERNAME = "deividjosue52@gmail.com"
        MAIL_PASSWORD = "brofrrjvuyvhqcfe"

        MODERATOR_DEFAULT_PASSWORD = "SecretPasswordM123"

        AZURE_STORAGE_ENABLED = "false"
        AZURE_STORAGE_CONNECTION_STRING = ""
        AZURE_STORAGE_CONTAINER_NAME = "imagenes"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/JosueGarciaAbata/marketplace_link_jenkins.git'
            }
        }

        stage('Build WAR') {
            steps {
                echo "Compilando backend..."
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Construyendo imagen Docker del backend..."
                sh "docker build -t ${DOCKER_IMAGE} ."
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
                  -p 8080:8080 \
                  -e SPRING_PROFILES_ACTIVE=${SPRING_PROFILES_ACTIVE} \
                  -e DB_HOST=${DB_HOST} \
                  -e DB_PORT=${DB_PORT} \
                  -e DB_NAME=${DB_NAME} \
                  -e DB_USER=${DB_USER} \
                  -e DB_PASSWORD=${DB_PASSWORD} \
                  ${DOCKER_IMAGE}
                """
            }
        }
    }
}
