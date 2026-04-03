pipeline {
    agent any // Выбираем Jenkins агента, на котором будет происходить сборка: нам нужен любой

    triggers {
        pollSCM('0 2 * * *') // Запускать будем автоматически по крону примерно раз в 5 минут
    }

    ; tools {
    ;     maven 'maven-3.8.1' // Для сборки бэкенда нужен Maven
    ;     //jdk 'jdk16' // И Java Developer Kit нужной версии
    ;     nodejs 'node-16' // А NodeJS нужен для фронта
    ; }

    stages {
        stage('Build') {
            steps {
                dir("code") { // Переходим в папку backend
                    sh 'mvn compile' // Собираем мавеном бэкенд
                }
            }

            post {
                success {
                    junit 'backend/target/surefire-reports/**/*.xml' // Передадим результаты тестов в Jenkins
                }
            }
        }
        
        ; stage('Save artifacts') {
        ;     steps {
        ;         archiveArtifacts(artifacts: 'code/target/*.jar')
        ;     }
        ;     post {
        ;             success { 
        ;                 sh """
        ;                 curl -X POST -H 'Content-type: application/json' \
        ;                 --data '{"chat_id": "-1002332977243", "text": "Никита Шаров собрал приложение." }' \
        ;                 https://api.telegram.org/bot5933756043:AAE8JLL5KIzgrNBeTP5e-1bkbJy4YRoeGjs/sendMessage """
        ;             }
        ;     }
        ; }
    }
}