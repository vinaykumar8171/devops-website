
pipeline {
    agent any

    stages {
        stage('Checkout Verification') {
            steps {
                echo 'Checking out the DevOps website source code'
                bat 'git --version'
                bat 'dir'
            }
        }

        stage('Validate Website') {
            steps {
                bat 'if not exist index.html (echo ERROR: index.html missing & exit /b 1)'
                bat 'if not exist style.css (echo ERROR: style.css missing & exit /b 1)'
                echo 'Website files found successfully'
            }
        }

        stage('Build Summary') {
            steps {
                echo 'Build completed successfully!'
                echo 'Next step: deploy this website with Docker'
            }
        }
    }

    post {
        success {
            echo 'CI Pipeline status: SUCCESS'
        }
        failure {
            echo 'CI Pipeline status: FAILURE - check Console Output'
        }
    }
}
