pipeline {
    agent any

    tools {
        jdk 'jdk21'
        maven 'maven3'
    }

    triggers {
        githubPush()
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
                echo 'Checking out code from GitHub...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running test cases...'
                sh 'mvn test'
            }
        }

        stage('Publish Test Report') {
            steps {
                echo 'Publishing JUnit test reports...'
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Generate HTML Report') {
            steps {
                echo 'Generating HTML report...'
                publishHTML(target: [
                    reportDir: 'target/site',
                    reportFiles: 'index.html',
                    reportName: 'Maven HTML Report',
                    keepAll: true,
                    alwaysLinkToLastBuild: true,
                    allowMissing: true
                ])
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo 'Archiving build artifacts...'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
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
            echo ' Cleaning workspace'
            cleanWs()
        }
    }
}
