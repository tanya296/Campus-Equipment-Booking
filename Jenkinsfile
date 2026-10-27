
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'feature/booking-state-consistency',
                    url: 'https://github.com/tanya296/Campus-Equipment-Booking.git'
            }
        }

        stage('Build and Test') {
            steps {
                bat 'mvn clean test'
            }
            post {
                always {
                    junit allowEmptyResults: true,
                          testResults: 'target/surefire-reports/*.xml'
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
