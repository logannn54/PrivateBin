pipeline {
    agent any
    environment {
        PHP_VERSION = "8.2"
    }
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }
        stage('Setup PHP & Dependencies') {
            steps {
                sh '''
                php -v
                composer install --no-interaction --prefer-dist
                '''
            }
        }
        stage('Run PHPCS') {
            steps {
                script {
                    try {
                        def phpcsResult = sh(script: './vendor/bin/phpcs --standard=phpcs.xml lib/', returnStatus: true)
                        if (phpcsResult != 0) {
                            currentBuild.result = 'UNSTABLE'
                            echo "ATTENTION: PHPCS violations found in lib/ directory!"
                        } else {
                            echo "PHPCS check passed successfully!"
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'UNSTABLE'
                        echo "ERREUR lors de l'exécution de PHPCS: ${e.message}"
                    }
                }
            }
        }
        stage('Run PHPMD') {
            steps {
                script {
                    try {
                        def phpmdResult = sh(script: './vendor/bin/phpmd lib/ text phpmd.xml', returnStatus: true)
                        if (phpmdResult != 0) {
                            currentBuild.result = 'UNSTABLE'
                            echo "ATTENTION: PHPMD issues detected in lib/ directory!"
                        } else {
                            echo "PHPMD check passed successfully!"
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'UNSTABLE'
                        echo "ERREUR lors de l'exécution de PHPMD: ${e.message}"
                    }
                }
            }
        }
        stage('Run PHPStan') {
            steps {
                script {
                    try {
                        def phpstanResult = sh(script: './vendor/bin/phpstan analyse --configuration=phpstan.neon lib/', returnStatus: true)
                        if (phpstanResult != 0) {
                            currentBuild.result = 'UNSTABLE'
                            echo "ATTENTION: PHPStan found issues in lib/ directory!"
                        } else {
                            echo "PHPStan check passed successfully!"
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'UNSTABLE'
                        echo "ERREUR lors de l'exécution de PHPStan: ${e.message}"
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline terminé. Consultez les logs pour voir les détails des erreurs éventuelles.'
        }
        unstable {
            echo 'Des problèmes ont été détectés mais le pipeline a continué son exécution.'
        }
        success {
            echo 'Tous les contrôles ont été passés avec succès!'
        }
        failure {
            echo 'Une erreur critique a empêché la terminaison du pipeline.'
        }
    }
}
