pipeline {
    agent none
    stages {
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"username",passwordVariable:"pass")]){
                node('jenkins-slave'){
                sh 'sudo chown -R jenkins:jenkins jobs'
                sh 'sudo chown -R jenkins:jenkins /var/lib/jenkins/'
                sh 'docker build . -t ${username}/jenkins_sprints:v1.0'
                sh 'docker login -u ${username} -p ${pass}'
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