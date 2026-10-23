

pipeline {
    agent any

    stages {
        stage('Validate Website') {
            steps {
                bat 'if not exist index.html (echo ERROR: index.html missing & exit /b 1)'
                bat 'if not exist style.css (echo ERROR: style.css missing & exit /b 1)'
                bat 'if not exist Dockerfile (echo ERROR: Dockerfile missing & exit /b 1)'

		bat 'findstr /i /c:"<html" index.html >nul || (echo ERROR: HTML tag missing & exit /b 1)'
		bat 'findstr /i /c:"<head" index.html >nul || (echo ERROR: HEAD tag missing & exit /b 1)'
		bat 'findstr /i /c:"<body" index.html >nul || (echo ERROR: BODY tag missing & exit /b 1)'

                echo 'Website files validated successfully'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t devops-website:1.0 .'
            }
        }

	stage('Validate Docker Image') {
    	    steps {
        	bat 'docker image inspect devops-website:1.0 >nul 2>&1 || (echo ERROR: Docker image missing & exit /b 1)'
        	echo 'Docker image validated successfully.'
    	    }
	}

        stage('Deploy Website') {
            steps {
                bat 'docker rm -f devops-website-container 2>nul & exit /b 0'
                bat 'docker run -d --restart unless-stopped --name devops-website-container -p 8081:80 devops-website:1.0'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'docker ps --filter name=devops-website-container'
                bat 'docker inspect -f "{{.State.Running}}" devops-website-container | findstr /i "true" >nul || (echo ERROR: Container is not running & exit /b 1)'
		echo 'Deployment verified: Container is running.'
            }
        }

        stage('HTTP Health Check') {
    	    steps {
                bat 'curl --fail --silent --show-error --retry 5 --retry-delay 2 http://localhost:8081 -o NUL'
                echo 'HTTP Health Check passed: Website returned a successful HTTP response.'
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

