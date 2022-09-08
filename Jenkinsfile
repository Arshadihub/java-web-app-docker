node{
    def buildNumber= BUILD_NUMBER
    stage("GIT CHECKOUT"){
        git 'https://github.com/Arshadihub/java-web-app-docker.git'
    }
    stage("MAVEN PACKAGE BUILD"){
     def mavenHome= tool name: "Maven",type: "maven"
     sh "${mavenHome}/bin/mvn clean package"
    }
    stage("DOCKER BUILD IMAGE"){
        
        sh "docker build -t arshadcsinfo/java-app:${buildNumber} ."
    }
    
    stage("DOCKER LOGIN AND PUSH"){
        withCredentials([string(credentialsId: 'Docker_Hub_Pass', variable: 'Docker_Hub_Pass')]){
        sh "docker login -u arshadcsinfo -p ${Docker_Hub_Pass}"
    }
        sh "docker push arshadcsinfo/java-app:${buildNumber}"
    }
    stage("DEPLOY APP AS DOCKER COTAINER"){
        sshagent(['Docker_Dev_Server_SSH']) {
        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.45.108 docker rm -f javawebcontainer || true"
        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.45.108 docker run -d -p 8080:8080 --name javawebcontainer arshadcsinfo/java-app:${buildNumber}" 
}
    }
    
}
