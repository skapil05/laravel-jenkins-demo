pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Downloading source code...'
                checkout scm
            }
        }

        stage('Composer Validate') {
            steps {
                sh 'composer validate'
            }
        }

        stage('Composer Install') {
            steps {
                sh 'composer install --no-interaction --prefer-dist'
            }
        }

        stage('Prepare Laravel') {
            steps {
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
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
        --exclude=vendor \
        ./ /var/www/laravel-demo/
        '''
             }
        }

        stage ('optimize laravel') {
         steps {
         sh '''
      cd /var/www/laravel-demo
      php artisan optimize:clear
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
        --exclude=vendor \
        ./ /var/www/laravel-production/
        '''
    }
}
    }
}
