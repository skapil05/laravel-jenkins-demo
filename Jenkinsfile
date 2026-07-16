pipeline {

    agent any
    environment {
        DEMO_KEY = credentials('demo-api-key')
}
    stages {

        stage('credentials Demo') {
          steps {
            sh'''
           echo "Credential loaded"
           echo $DEMO_KEY
}
}
        stage('Checkout') {
            steps {
                echo 'Downloading latest source code...'
                checkout scm
            }
        }

        stage('Composer Validate') {
            steps {
                sh 'composer validate'
            }
        }

        stage('Composer Install (Build)') {
            steps {
                sh 'composer install --no-interaction --prefer-dist'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'php artisan test'
            }
        }

        stage('Deploy Staging') {
            steps {
                sh '''
                rsync -av --delete \
                --exclude=.git \
                --exclude=.env \
                --exclude=vendor \
                ./ /var/www/laravel-demo/

                cd /var/www/laravel-demo

                composer install --no-dev --optimize-autoloader

                php artisan migrate --force

                php artisan optimize
                '''
            }
        }

        stage('Approval') {
            steps {
                input 'Deploy to Production?'
            }
        }

        stage('Deploy Production') {
            steps {
                sh '''
                rsync -av --delete \
                --exclude=.git \
                --exclude=.env \
                --exclude=vendor \
                ./ /var/www/laravel-production/

                cd /var/www/laravel-production

                composer install --no-dev --optimize-autoloader

                php artisan migrate --force

                php artisan optimize
                '''
            }
        }
    }
}
