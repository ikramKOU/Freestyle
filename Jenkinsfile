pipeline {
    agent any

    tools {
        jdk 'Java17'
        maven 'Maven'
    }

    triggers {
        pollSCM('H/5')
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Récupération du code source'
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Compilation et exécution des tests'
                bat 'java -version'
                bat 'mvn -version'
                bat 'mvn clean package'
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESS ✅'
        }
        failure {
            echo 'BUILD FAILED ❌'
        }
    }
}
