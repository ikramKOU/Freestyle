// pipeline {
//     agent any

//     tools {
//         jdk 'Java17'
//         maven 'Maven'
//     }

//     triggers {
//         pollSCM('H/5 * * * *')
//     }

//     stages {

//         stage('Checkout') {
//             steps {
//                 echo 'Récupération du  source'
//                 checkout scm
//             }
//         }
// // ci 
//         stage('Build & Test') {
//             steps {
//                 echo 'Compilation et exécution des tests'
//                 bat 'java -version'
//                 bat 'mvn -version'
//                 bat 'mvn clean package'
//             }
//         }
//     }

//     post {
//         success {
//             echo 'BUILD SUCCESS'
//         }
//         failure {
//             echo 'BUILD FAILED'
//         }
//     }
// }
pipeline {
    agent any

    tools {
        jdk 'Java17'
        maven 'Maven'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Récupération du code source'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Compilation du projet'
                bat 'java -version'
                bat 'mvn -version'
                bat 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Exécution des tests unitaires'
                bat 'mvn test'
            }
            post {
                always {
                    // Affiche les résultats des tests JUnit dans Jenkins
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build réussi, tests OK'
        }
        failure {
            echo '❌ Build échoué'
        }
    }
}
