pipeline {
    agent any

    environment {
        DOCKER_IMAGE     = "myapp:${env.BUILD_NUMBER}"
        SONAR_PROJECT_KEY = "my-org_my-repo"
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timeout(time: 30, unit: 'MINUTES')
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out source code..."
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube analysis..."
                script {
                    withSonarQubeEnv('sonarqube ') {
                        sh '''
                            sonar-scanner \
                              -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                              -Dsonar.sources=. \
                              -Dsonar.host.url=${SONAR_HOST_URL} \
                              -Dsonar.login=${SONAR_AUTH_TOKEN}
                        '''
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image: ${DOCKER_IMAGE}"
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            when {
                branch 'main'
            }
            steps {
                echo "Pushing ${DOCKER_IMAGE} to registry..."
                script {
                    docker.withRegistry('https://registry.example.com', 'docker-registry-cred-id') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished with status: ${currentBuild.currentResult}"
        }
    }
}