pipeline {
    agent any
    tools {
        maven 'm3' 
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        SONAR_URL = 'http://13.233.117.60:9000'
        SONAR_LOGIN = 'squ_4aa21968c9c5793c9448079146ee5f14e0d7e19d'
        SONAR_PROJECT_NAME = 'medicare'
        SONAR_PROJECT_KEY = 'medicare'
    }
    stages {
        stage('checkout') {
            steps {
                git 'https://github.com/Sharuqmd/Healthcare-project.git' 
            }
        }
        stage('compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('sonar-analysis') {
            steps {
                sh '''${SCANNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                    -Dsonar.sources=. \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectName=${SONAR_PROJECT_NAME} \
                    -Dsonar.host.url=${SONAR_URL} \
                    -Dsonar.login=${SONAR_LOGIN}'''
            }
        }
        stage('oswap-scan') {
            steps {
                sh 'mvn clean install'
            }
        } 
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        } 
        stage('Build and push Docker image') {
            steps {
               script {
                  withDockerRegistry(url: 'https://hub.docker.com/repository/docker/sharuq/medicare') {
                      sh 'docker build -t medicareapp .'
                      sh 'docker tag sharuq/medicare:latest'
                      sh 'docker push sharuq/medicare:latest'
}
               }
            }
        }         
    }
}
