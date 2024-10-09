pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
        registry = '654654271537.dkr.ecr.us-east-1.amazonaws.com/devops-repository'
        registryCredential = 'jenkins-ecr'
        dockerimage = ''
    }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/CathyYolande/Helloworld.git'
            }
        }
        stage ("Sonarqube scan") {
            steps{
                withSonarQubeEnv('sonarQube') {
                    sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=CathyYolande_geolocation'
                }
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}