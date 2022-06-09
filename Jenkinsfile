pipeline {
    agent none
    stages {
        stage('Build') {
            steps {
                
                // Get some code from a GitHub repository
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"username",passwordVariable:"pass")]){
                node('jenkins-slave'){
                sh 'sudo docker build . -t ${username}/jenkins_sprints:v1.0'
                sh 'sudo docker login -u ${username} -p ${pass}'
                sh 'sudo docker push ${username}/jenkins_sprints:v1.0'
                }
            }
            }
        } 
        stage ('deploy'){
            steps{
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"username",passwordVariable:"pass")]){
                node('jenkins-slave'){
                sh 'sudo docker run -p 3000:3000 -d ${username}/jenkins_sprints:v1.0'
                }
            }
            }
        }
    }
} 