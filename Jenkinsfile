pipeline {
    agent none
    environment {
        REDIS_SERVER_FQDN = "localhost"
        REDIS_SERVER_PORT = "3000"
        REDIS_USER =  "root"
        REDIS_PASS = "root"
    }
    stages {
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"username",passwordVariable:"pass")]){
                node('jenkins-slave'){
                git branch: 'rds_redis', url: "https://github.com/nouranhamdy/jenkins_nodejs_example.git" 
                sh 'sudo chmod 777 /var/run/docker.sock'
                sh 'docker login -u ${username} -p ${pass}'
                sh 'docker build . -f dockerfile -t ${username}/jenkins_sprints:v1.0'
                sh 'docker push ${username}/jenkins_sprints:v1.0'
                }
            }
            }
        } 
        stage ('deploy'){
            steps{
                withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"username",passwordVariable:"pass")]){
                node('jenkins-slave'){
                sh 'docker run -p 3000:3000 --privileged=true --env-file=/var/jenkins_home/nodeapp/env.list -d ${username}/jenkins_sprints:v1.0'
                }
            }
            }
        }
    }
} 