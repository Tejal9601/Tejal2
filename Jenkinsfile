pipeline {
    agent any

    tools {
        jdk 'jdk21'
        maven 'maven3'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url:'https://github.com/Tejal9601/Jen_AutoTriggerP.git'
            }
        }

        stage('Verify Tools') {
            steps {
                bat 'java -version'
                bat 'mvn -version'
            }
        }

        stage('Clean Project') {
            steps {
                bat 'mvn clean'
            }
        }

        stage('Run Automation Tests') {
            steps {
                bat 'mvn test'
            }
        }
    }

    post {

        always {

            // Publish JUnit XML Results
            junit '**/target/surefire-reports/*.xml'

            // Publish HTML Report
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'target/surefire-reports',
                reportFiles: 'index.html',
                reportName: 'Automation HTML Report'
            ])
        }

        success {
            echo '✅ Automation Tests Passed Successfully!'
        }

        failure {
            echo '❌ Automation Tests Failed!'
        }
    }
}
