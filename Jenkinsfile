pipeline {
    agent any

    environment {
        // Set environment variables
        COMPOSER_HOME = '/var/www/.composer'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from repository
                git branch: 'main', url: 'https://github.com/suryaeoxys/laravel.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install PHP dependencies
                sh 'composer install'
                // Install Node.js dependencies
                sh 'npm install'
            }
        }
            }
        }

        stage('Build Assets') {
            steps {
                // Build front-end assets
                sh 'npm run production'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the application
                sshagent (credentials: ['SSH_KEYS']) {
                    sh '''
                        rsync -avz --delete --exclude="storage/" --exclude=".env" --exclude="vendor/" ./ azureuser@20.106.169.247:/var/www/laravel/
                        ssh azureuser@20.106.169.247 "cd /var/www/laravel && composer install --no-dev && php artisan migrate --force"
                    '''
                }
            }
        }
    }

    post {
        always {
            // Cleanup actions or notifications
        }
    }
}
