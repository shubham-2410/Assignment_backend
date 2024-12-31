pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        SONARQUBE_TOKEN = credentials('sonarqube-token') 
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    bat 'npm install'
                }
            }
        }

        // stage('Run Linter') {
        //     steps {
        //         bat 'npm run lint'
        //     }
        // }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') { // Replace 'SonarQube' with your server configuration name
                    bat """
                    sonar-scanner ^
                        -Dsonar.projectKey=Assignment2_ShubhamPhalke ^
                        -Dsonar.sources=. ^
                        -Dsonar.exclusions=node_modules/** ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.token=%SONARQUBE_TOKEN%
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully."
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
