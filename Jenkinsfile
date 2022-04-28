pipeline{
    agent any
     tools {
      maven 'maven-3.8.5'
      }
    
    stages{
        stage("SCM"){
            steps{
                git credentialsId: 'Git_credential',
                url: 'https://github.com/Raghav8125/dockeransiblejenkins'
            }
        } 
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Docker Build"){
            steps{
                sh "docker build -t raghav8125/prodevans:0.0.1 ."
            }
        }
        stage("Docker push"){
            steps{
                withCredentials([string(credentialsId: 'docker-Hub', variable: 'dockerHubpwd')]) {
                    sh "docker login -u raghav8125 -p ${dockerHubpwd}"
                 }
               
                sh "docker push raghav8125/prodevans:0.0.1 "
            }
        }
        
        stage("Docker Deploy"){
            steps{
                ansiblePlaybook credentialsId: 'dev-server', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
            }
        }   
        
    }
}
