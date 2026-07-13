pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Downloading source code...'
                checkout scm
            }
        }

        stage('Workspace') {
            steps {
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('PHP') {
            steps {
                sh 'php -v'
            }
        }

    }

}
