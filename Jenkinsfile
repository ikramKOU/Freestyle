
pipeline {
    agent any

    tools {
        jdk 'Java17'
        maven 'Maven'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'ikram last  UUyy Récupération du code source'
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                echo '--- haha ikjjjjjramm ------Compilation et exécution des tests'
                sh 'java -version'
                sh 'mvn -version'
            }
        }
    }

    post {
        success {
            echo 'BUILD ------------------SUCCESS '
        }
        failure {
            echo 'BUILD FAILED '
        }
    }
}
