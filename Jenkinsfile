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
        SONAR_PROJECT_KEY = "tp4-java-project"

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

        stage('Test GitHub Credential') {
            steps {
                withCredentials([
                    string(credentialsId: 'tokentp4', variable: 'GITHUB_TOKEN')
                ]) {
                    bat 'echo Token  mais masqué dans les logs'
                }
            }
        }

        stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('SonarQube') {
        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
    bat "mvn sonar:sonar -Dsonar.login=%SONAR_TOKEN%"
}

    }
}




        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
// OUTIL LI KAYCHOF PROJET AKML KAYVERIFIER LIH LES TEST  /// jacoco ;;; il faut le cond=fugurer 
        stage('Docker Build') {
            steps {
                echo 'Constr uction de l’image Docker'
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Deploy (Local Docker)') {
            steps {
                echo 'Déploiement  conteneur Docker'
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
            echo ' TP4 réussi : build, tests, SonarQube et Quality Gate OK'
        }
        failure {
            echo ' Pipeline bloqué : erreur ou Quality Gate FAIL'
        }
    }
}
