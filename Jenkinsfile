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
                    junit 'code/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh './code/scripts/deliver.sh' 
            }
        }
    }
}