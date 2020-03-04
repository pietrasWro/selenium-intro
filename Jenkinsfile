pipeline {
    agent any
    stages {
        stage('Build test code') {
            steps {
                bat 'mvn clean install -DskipTests' // Budowanie testów
            }
        }
        stage('Run selenium grid') {
            steps {
                bat 'docker-compose up -d' // Uruchiomienie Docker Selenium
            }
        }
        stage('Execute test') {
            steps {
                bat 'mvn test' // Uruchomienie testów
                bat 'docker-compose down' // Wyłączenie Docker Selenium, wyłączenie kontenerów
            }
        }
    }
    post {
        always {
            script { // Wygenerowanie raportu allurowego
                allure([
                        includeProperties: false,
                        jdk              : '',
                        properties       : [],
                        reportBuildPolicy: 'ALWAYS',
                        results          : [[path: 'target/allure-results']]
                ])
            }
        }
    }
}