
pipeline {
    agent any

    stages {
        stage('Checkout Verification') {
            steps {
                echo 'Checking DevOps website source code'
                bat 'git --version'
                bat 'dir'
            }
        }

        stage('Validate Website') {
            steps {
                bat 'if not exist index.html (echo ERROR: index.html missing & exit /b 1)'
                bat 'if not exist style.css (echo ERROR: style.css missing & exit /b 1)'
                bat 'if not exist Dockerfile (echo ERROR: Dockerfile missing & exit /b 1)'
                echo 'Website files and Dockerfile found'
            }
        }

        stage('Docker Environment Check') {
            steps {
                bat 'docker --version'
                bat 'docker version'
                bat 'docker info'
            }
        }
    }

    post {
        success {
            echo 'CI Pipeline and Docker environment check: SUCCESS'
        }
        failure {
            echo 'Pipeline failed. Check Console Output for the first error.'
        }
    }
}
