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

    environment {
        IMAGE_NAME = "tp3-java-app:latest"
        CONTAINER_NAME = "tp3-java-container"
        HOST_PORT = "8081"
        CONTAINER_PORT = "8080"
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
                bat 'mvn clean package -DskipTests'
            }
        } 

        stage('Test') {
            steps {
                echo 'Exécution des tests'
                bat 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Construction de l’image Docker'
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Deploy (Local Docker)') {
            steps {
                echo 'Déploiement du conteneur Docker'
                bat '''
                docker stop %CONTAINER_NAME% || exit 0
                docker rm %CONTAINER_NAME% || exit 0
                docker run -d --name %CONTAINER_NAME% -p %HOST_PORT%:%CONTAINER_PORT% %IMAGE_NAME%
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Déploiement local terminé avec succès'
        }
        failure {
            echo '❌ Erreur dans le pipeline'
        }
    }
}
