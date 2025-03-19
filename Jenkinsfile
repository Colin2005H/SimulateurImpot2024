pipeline {
    agent any

    environment {
        // Définir des variables d'environnement si nécessaire
        APP_NAME = 'simulateur-impot-2024'
        BUILD_DIR = 'build'
    }

    stages {
        stage('Checkout') {
            steps {
                // Cloner le dépôt de code source
                git 'https://github.com/Colin2005H/SimulateurImpot2024.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Exécuter les commandes de build
                    sh './gradlew build' // Pour un projet Gradle
                    // ou
                    // sh 'mvn clean install' // Pour un projet Maven
                }
            }
            post {
                success {
                    echo 'Build réussi.'
                }
                failure {
                    echo 'Échec du build.'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Exécuter les tests unitaires
                    sh 'mvn test'
                }
            }
            post {
                always {
                    junit '**/build/test-results/**/*.xml'
                }
                success {
                    echo 'Tests réussis.'
                }
                failure {
                    echo 'Échec des tests.'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Exécuter les commandes de déploiement
                    sh './scripts/deploy.sh'
                }
            }
            post {
                success {
                    echo 'Déploiement réussi.'
                }
                failure {
                    echo 'Échec du déploiement.'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline terminé avec succès.'
        }
        failure {
            echo 'Échec du pipeline.'
        }
    }
}
