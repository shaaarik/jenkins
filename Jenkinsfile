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
            //     success {
            //         echo "✅ Успех! Ветка ${env.BRANCH_NAME} собралась и тесты пройдены."
            //         // Опционально: отметить commit статусом success в GitHub
            //         // githubNotify context: 'Jenkins CI', status: 'SUCCESS'
            //     }
            //     unstable {
            //         echo "⚠️ Нестабильно! Тесты упали, но сборка завершилась."
            //         // githubNotify context: 'Jenkins CI', status: 'FAILURE'
            //     }
            //     failure {
            //         echo "❌ Критическая ошибка! Ветка ${env.BRANCH_NAME} не собралась."
            //         // githubNotify context: 'Jenkins CI', status: 'FAILURE'
            //     }
            // }
        }
    }
}