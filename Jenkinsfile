pipeline {
    agent any

    tools {
        jdk 'jdk21'
        maven 'maven3'
    }

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    environment {
        MAVEN_OPTS = '-Dmaven.test.failure.ignore=false'
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                bat 'mvn clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running test cases...'
                bat 'mvn test'
            }
        }

        stage('Publish Test Report') {
            steps {
                echo 'Publishing JUnit test report...'
                junit allowEmptyResults: true,
                      testResults: 'target/surefire-reports/*.xml'
            }
        }
    }

    post {
        success {
            echo ' Build completed successfully'
        }
        failure {
            echo ' Build failed'
        }
        always {
            echo 'Cleaning workspace'
            cleanWs()
        }
    }
}
