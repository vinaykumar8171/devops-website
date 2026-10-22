

pipeline {
    agent any

    stages {
        stage('Validate Website') {
            steps {
                bat 'if not exist index.html (echo ERROR: index.html missing & exit /b 1)'
                bat 'if not exist style.css (echo ERROR: style.css missing & exit /b 1)'
                bat 'if not exist Dockerfile (echo ERROR: Dockerfile missing & exit /b 1)'
                echo 'Website files validated successfully'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t devops-website:1.0 .'
            }
        }

        stage('Deploy Website') {
            steps {
                bat 'docker rm -f devops-website-container 2>nul & exit /b 0'
                bat 'docker run -d --name devops-website-container -p 8081:80 devops-website:1.0'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'docker ps --filter name=devops-website-container'
                echo 'Deployment command completed. Open http://localhost:8081 to verify.'
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output for the first error.'
        }
    }
}

