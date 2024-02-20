pipeline{
    agent any
    tools {
      maven 'maven3'
    }
    
    environment {
      DOCKER_TAG = getVersion()
    }
    
    stages{
        stage('SCM'){
            steps{
                git branch: 'main', credentialsId: 'githubdockeransiblejenkins', 
                    url: 'https://github.com/anjankdey18/dockeransiblejenkins.git'
            }
        }
        
        stage('Maven Build'){
            steps{
                sh "mvn clean package"
            }
        }
        
        stage('Docker Build'){
            steps{
                sh "docker build . -t anjandey18/mymvnimage:${DOCKER_TAG} "
            }
        }
                        
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u anjandey18 -p ${dockerHubPwd}"
                }
                sh "docker push anjandey18/mymvnimage:${DOCKER_TAG} "
            }
        }
        
        stage('Docker Deploy'){
            steps{
                ansiblePlaybook credentialsId: 'docker-server-access-dev', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${DOCKER_TAG}", installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml', vaultTmpPath: ''
            }
        }
    }
}

def getVersion(){
    def commitHash = sh lavel: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
