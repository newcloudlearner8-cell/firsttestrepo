pipeline {
    agent any

    tools {
        maven 'Maven-3.9.6'
    }

    environment {
        IMAGE_NAME = "firsttestrepo-app"
        IMAGE_TAG  = "latest"
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building Java project...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
                junit allowEmptyResults: true,
                      testResults: 'target/surefire-reports/*.xml'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh '''
                  docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Docker Image Info') {
            steps {
                sh 'docker images | grep ${IMAGE_NAME}'
            }
        }
    }

    post {
        success {
            echo 'Java + Maven + Docker pipeline completed successfully 🎉'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}
