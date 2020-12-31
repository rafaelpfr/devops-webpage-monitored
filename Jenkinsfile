pipeline {
  agent any 

  triggers {
    pollSCM('* * * * *')
  }
  
  environment{
   IMAGE_TAG = getVersion()
  }
  
  stages{
    
    stage('Build Docker Image') {
      steps {
        sh 'docker image build -t rafa73/static_site:${IMAGE_TAG} --network=host .'
      }
   }


    stage('Push image to Docker Hub') {

      steps {
        withCredentials([string(credentialsId: 'docker-hub-password', variable: 'dockerHubPassword')]) {
          sh 'docker login -u rafa73 -p ${dockerHubPassword}'
      }
       sh 'docker push rafa73/static_site:${IMAGE_TAG}'
      }
   }

    stage('Install community.kubernetes chart') {
       steps {
         sh 'ansible-galaxy collection install community.kubernetes'
       }
    }
  
    stage('Ansible Event') {
      steps {
        ansiblePlaybook credentialsId: 'dev-server', disableHostKeyChecking: true, extras: "-e IMAGE_TAG=${IMAGE_TAG}", installation: 'Ansible-2.9.15', inventory: 'hosts', playbook: 'playbook.yml'
      }
    } 
  }
}

def getVersion(){
  def commitHash = sh returnStdout: true, script: 'git rev-parse --short HEAD'
  return commitHash 
}

