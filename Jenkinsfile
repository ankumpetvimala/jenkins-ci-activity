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
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Validation') {
            steps {
                bat 'if exist pom.xml (echo pom.xml found) else (echo pom.xml missing & exit /b 1)'
                bat 'if exist target (echo target folder found) else (echo target folder missing & exit /b 1)'
                bat 'if exist src\\main\\java\\App.java (echo App.java found) else (echo App.java missing & exit /b 1)'
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
