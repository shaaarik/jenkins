pipeline {
    agent any

    triggers {
        pollSCM('0 9 * * *') 
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
        stage('Save artifacts') {
            steps {
                archiveArtifacts(artifacts: 'code/target/*.jar')
            }
            // post {
            //         success { 
            //             sh """
            //             curl -X POST -H 'Content-type: application/json' \
            //             --data '{"chat_id": "-8572143233", "text": "Никита Шаров собрал приложение." }' \
            //             https://api.telegram.org/bot8572143233:AAGYx1PLrK37_8AJzP_66tfXdUjA8iUYon8/sendMessage """
            //         }
            // }
        }
    }
}