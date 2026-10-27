
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is available in the workspace'
            }
        }

        stage('Build and Test') {
            steps {
                bat 'mvn clean test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package'
                archiveArtifacts artifacts: 'target/*.jar',
                                 fingerprint: true
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t campus-equipment-booking:set-f .'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
