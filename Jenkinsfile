pipeline {

    agent any

    tools {
        sonarQube 'SonarScanner'
    }

    environment {
        IMAGE_NAME = "sample-app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YOUR_USERNAME/YOUR_REPO.git',
                    credentialsId: 'github-token'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {

                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=sample-app \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://sonarqube:9000 \
                    -Dsonar.login=$SONAR_AUTH_TOKEN
                    '''

                }
            }
        }

        stage('Build Docker Image') {

            steps {

                sh '''
                docker build -t $IMAGE_NAME .
                '''

            }

        }

    }

    post {

        success {

            echo "Pipeline Successful"

        }

        failure {

            echo "Pipeline Failed"

        }

    }

}