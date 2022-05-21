pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/muhammedgamal760/jenkins_nodejs_example.git'
            }
        }
        stage('docker build'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                sh "docker login -u ${env.DOCKER_USER} -p ${env.DOCKER_PASS}"}
                //building docker file
                sh 'docker build . -t muhammedgamal/jenkins'
            }
        }
        stage('docker push'){
            steps{
                //adding credentials
                
                //pushing image
                sh 'docker push muhammedgamal/jenkins'
            
        }
        }
        stage('docker run'){
            steps{
                //run docker
                sh 'docker run -p 3000:3000 muhammedgamal/jenkins'
            }
        }
        
    }
}
