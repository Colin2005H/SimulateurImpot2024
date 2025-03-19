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
                    bat './gradlew build' // Remplacez par votre commande de build
                }
            }
            post {
                failure {
                    script {
                        // Copier les fichiers HTML et MP3 dans le répertoire de travail
                        bat 'cp padoru-smooth.gif .'
                        //sh 'cp path/to/your/techno.mp3 .'

                        // Publier le fichier HTML
                        publishHTML target: [
                            reportDir: '.',
                            reportFiles: 'error.html',
                            reportTitle: 'Erreur de Build'
                        ]

                        // Jouer le son (si le plugin Audio Notification est configuré)
                        //audioNotification soundFile: 'techno.mp3'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Exécuter les tests unitaires
                    bat 'mvn test'
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
                    bat './scripts/deploy.sh'
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

