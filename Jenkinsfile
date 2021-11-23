node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/Mayank07-pixel/DockerApp.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.5.6", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t Mayank07-pixel/DockerApp .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u Mayank07-pixel -p ${Docker_Hub_Pwd}"
        }
        sh 'docker push  Mayank07-pixel/DockerApp'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app  Mayank07-pixel/DockerApp'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.20.72 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.20.72 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.20.72 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.20.72 ${dockerRun}"
       }
       
    }
     
     
}
