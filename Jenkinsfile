pipeline {
    agent any

    triggers {
        pollSCM('0 2 * * *') 
    }

    tools {
        maven 'Maven-3'
    }
    stages {
        stage('Build') {
            steps {
                dir("code") { 
                    sh 'mvn -B -DskipTests clean package' 
                }
            }
        }
        stage('Test') {
            steps {
                dir("code") {
                    sh 'mvn test'
                }
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}