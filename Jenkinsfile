pipeline {
    agent none
    stages {
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"username",passwordVariable:"pass")]){
                node('jenkins-slave'){
                git 'https://github.com/muhammedgamal760/jenkins_nodejs_example.git'
                sh 'sudo chmod 777 /var/run/docker.sock'
                sh 'docker login -u ${username} -p ${pass}'
                sh 'docker build . -f ../dockerfile -t ${username}/jenkins_sprints:v1.0'
                sh 'docker push ${username}/jenkins_sprints:v1.0'
                }
            }
            }
        } 
        stage ('deploy'){
            steps{
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"username",passwordVariable:"pass")]){
                node('jenkins-slave'){
                sh 'docker run -p 3000:3000 -d ${username}/jenkins_sprints:v1.0'
                }
            }
            }
        }
    }
} 