-pipeline {
    agent any

    environment {
        PATH = "$PATH:/usr/local/bin"
    }

    stages {
        stage("Sonar Scan") {
            when {
                branch "master"
            }
            environment {
                SCANNER_HOME = tool 'SonarQubeServer'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    echo "not here"
                    sh "${SCANNER_HOME}/bin/sonar-scanner"
                    echo "here"
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
