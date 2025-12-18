pipeline {
    agent any

    tools {
        maven 'Maven-3.9.6'   // Configure this name in Jenkins
        jdk 'JDK-17'          // Configure this name in Jenkins
    }

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Compiling Java source code...'
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging application...'
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Java + Maven build completed successfully 🎉'
        }
        failure {
            echo 'Build failed ❌'
        }
        always {
            echo 'Pipeline finished.'
        }
    }
}
