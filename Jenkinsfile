pipeline {  
    agent any
    stages {
        stage('Build') {
            agent {
                docker { image 'maven:latest'
                 args '-u root'
                }   
            }
            steps {
               sh 'mvn clean package'
            }
        }
        stage('Static Code Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_TOKEN')]) {
                    sh 'mvn sonar:sonar -Dsonar.host.url=http://13.232.158.43:9000 -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }        
        stage('Build & Push Image'){
            agent any
            environment{
                //IMAGE = "ashudhub/mvnwebapp:${BUILD_NUMBER}"
                REGISTRY_CREDENTIALS = credentials('docker')
            }
            steps{
                script {
                		sh 'docker build -t ashudhub/mvnwebapp:latest .'
                        //def dockerImage = docker.image("${IMAGE}")
                        docker.withRegistry('https://index.docker.io/v1/', 'docker') {
                        docker.image("ashudhub/mvnwebapp:latest").push() }
                        
                }
            }
        }
    }
}
