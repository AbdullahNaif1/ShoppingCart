pipeline {
    agent any

    environment {
        // Define any environment variables here, if needed
        DOCKER_IMAGE = 'shoppingcart_app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Build the application using Maven
                sh 'mvn clean install' 
            }
        }

        stage('Test') {
            steps {
                // Run your tests here, for example using Maven
                sh 'mvn test' 
            }
        }

        stage('Docker Build') {
            steps {
                // Build a Docker image
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                // Push the Docker image to a registry
                script {
                    docker.withRegistry('https://your-registry-url', 'your-credentials-id') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Docker Deploy') {
            steps {
                // Deploy using Docker
                sh "docker service update --image ${DOCKER_IMAGE} your_service_name || docker service create --name your_service_name ${DOCKER_IMAGE}"
            }
        }
    }
}
