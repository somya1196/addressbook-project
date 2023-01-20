node {
    def docker
    def dockerCMD
    
    stage ('Praparation for jenkins') {
        echo 'initializing docker tool'
        docker= tool name: 'docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
        dockerCMD= "${docker}/bin/docker"
    }
    
    
    stage ('Git checkout') {
        git 'https://github.com/somya1196/addressbook-project.git'
    }
    
    stage ('Clean package') {
        sh 'mvn clean package'
    }
    
    stage ('Building docker image') {
        echo 'Building docker image from dockerfile'
        sh "${dockerCMD} build -t somya1196/addressbook:8.2 ."
         }
         
    stage ('Pushing image to Dockerhub') {
        withCredentials([string(credentialsId: 'dockerHubPwd', variable: 'dockerHub')]) {
          sh "${dockerCMD} login -u somya1196 -p ${dockerHub}"
          sh "${dockerCMD} push somya1196/addressbook:8.2 "
         }
       }
       
       stage ('Configure Deployment server and deploy') {
           ansiblePlaybook credentialsId: 'ansible-ssh', disableHostKeyChecking: true, inventory: '/etc/ansible/hosts', playbook: 'configure-server.yml'
           }
      }  
