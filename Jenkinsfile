pipeline {
    agent any

    environment {
        PATH = "$PATH:/usr/local/bin"
    }

    stages {
        stage("Sonar Scan") {
            when {
                branch "master"
            }
            steps {
                script {
                    scannerHome = tool 'SonarQube';
                }
                withSonarQubeEnv('SonarQubeServer') {

                    sh """${scannerHome}/bin/sonar-scanner -Dsonar.login=${env.SONAR_AUTH_TOKEN}"""
                }
            }
        }
        stage("Deploy Prod") {
            when {
                branch "master"
            }
            steps {
                echo "Deploying and Building..."
                sh "docker-compose build"
                echo "Stopping previous container..."
                sh "docker-compose down"
                sh "docker-compose up -d"
                echo "Deployed!"
            }
        }
    }
}
