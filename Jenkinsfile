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
                    withMaven(maven: 'mvn') {
                        sh "mvn clean deploy sonar:sonar"
                    }
//                     sh """${scannerHome}/bin/sonar-scanner -Dsonar.login=admin -Dsonar.password=qweqwe"""
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
