pipeline {
    agent any // Выбираем Jenkins агента, на котором будет происходить сборка: нам нужен любой

    triggers {
        pollSCM('0 2 * * *') // Запускать будем автоматически по крону примерно раз в 5 минут
    }

    tools {
        maven 'Maven-3'
    }

    stages {
        stage('Build') {
            steps {
                dir("code") { // Переходим в папку backend
                    sh 'mvn compile' // Собираем мавеном бэкенд
                }
            }

            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' // Передадим результаты тестов в Jenkins
                }
            }
        }
    }
}