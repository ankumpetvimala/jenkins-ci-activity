pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Validation') {
            steps {
                sh 'test -f pom.xml'
                sh 'test -f src/main/java/App.java'
                echo 'Additional validation completed successfully.'
            }
        }
    }

    post {
        success {
            echo 'CI Pipeline completed successfully.'
        }

        failure {
            echo 'CI Pipeline failed. Check the Console Output.'
        }
    }
}
