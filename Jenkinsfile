pipeline {
    agent any

    environment {
        // Define any environment variables here, if needed
        DOCKER_IMAGE = 'shoppingcart_app'
    }

    stages {
        stage('Checkout') {
            steps {
                // Get the code from the GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/AbdullahNaif1/ShoppingCart.git']]])
            }
        }

        stage('Build') {
            steps {
                // Build the application. Replace this with your build command if it's different.
                sh 'make build' // Assuming a Makefile is present with a 'build' target
            }
        }

        stage('Test') {
            steps {
                // Run your tests here. Replace this with your test command if it's different.
                sh 'make test' // Assuming a Makefile is present with a 'test' target
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
                // Push the Docker image to a registry. Make sure to replace 'your-registry-url' and 'your-credentials-id'.
                script {
                    docker.withRegistry('https://your-registry-url', 'your-credentials-id') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Docker Deploy') {
            steps {
                // Deploy using Docker. Adjust the service name and other settings as needed.
                sh "docker service update --image ${DOCKER_IMAGE} your_service_name || docker service create --name your_service_name ${DOCKER_IMAGE}"
            }
        }
    }
}
