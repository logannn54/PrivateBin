pipeline {
    agent any

    environment {
        PHP_VERSION = "8.2"
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
                    def phpcsResult = sh(script: './vendor/bin/phpcs --standard=phpcs.xml lib/', returnStatus: true)
                    if (phpcsResult != 0) {
                        error("PHPCS violations found in lib/ directory!")
                    }
                }
            }
        }

        stage('Run PHPMD') {
            steps {
                script {
                    def phpmdResult = sh(script: './vendor/bin/phpmd lib/ text phpmd.xml', returnStatus: true)
                    if (phpmdResult != 0) {
                        error("PHPMD issues detected in lib/ directory!")
                    }
                }
            }
        }

        stage('Run PHPStan') {
            steps {
                script {
                    def phpstanResult = sh(script: './vendor/bin/phpstan analyse --configuration=phpstan.neon lib/', returnStatus: true)
                    if (phpstanResult != 0) {
                        error("PHPStan found issues in lib/ directory!")
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé.'
        }
        failure {
            echo 'Une ou plusieurs erreurs ont été détectées dans lib/ !'
        }
    }
}

