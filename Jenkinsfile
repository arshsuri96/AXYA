node
{
    def buildNumber = BUILD_NUMBER
  stage("CheckOutCodeGit")
  {
   git 'https://github.com/arshsuri96/nodejs.git'
 }
 
 stage("Build Docker Image")
 {
    sh "docker build -t arsh1/k8:${buildNumber} ."
 }
 
  stage("Build Login and Push")
 {
    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
        sh "docker login -u arsh1 -p ${dockerhub}"
}
    sh "docker push arsh1/k8:${buildNumber}"
 }
 
 stage("deploy application as docker"){
     
     def dockerRun = 'docker run -d --name nodejsapp -p 3000:3000 arsh1/k8:${buildNumber}'
    sshagent(['dev-nodejs']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.47.139 ${dockerRun}'
    }
 }
}