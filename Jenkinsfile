pipeline {
    agent any
    tools {
       maven 'maven 3.8.6'
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                echo 'hi'
            }
        }
        stage ('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/cbabu85/sprint-petclinic-latest.git']]])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('sonarqube checks') {
            steps {
                script {
                withSonarQubeEnv(installationName: 'sonarqube-1', credentialsId: 'jenkins-sonar-token') {
                 sh 'mvn clean package sonar:sonar'
                 }
                    timeout(time: 1, unit: 'HOURS') {
                       def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                       error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                }
            }
        }
    }
}
}
