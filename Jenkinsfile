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
                sh 'docker run -p 3000:3000 --privileged=true -e RDS_HOSTNAME="jimmy.cdmkspliwudw.eu-central-1.rds.amazonaws.com" -e RDS_USERNAME="jimmy" -e RDS_PASSWORD="jimmyyyy" -e RDS_PORT="3306" -e REDIS_HOSTNAME="jimmyredis.fiqmwz.0001.euc1.cache.amazonaws.com:6379" -e REDIS_PORT="6379" -d ${username}/jenkins_sprints:v1.0'
                }
            }
            }
        }
    }
} 