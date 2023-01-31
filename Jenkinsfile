pipeline {
    agent any 
    environment {
        dockerImage = ''
        registry = "rutujapawal/mypythonapp"
        registryCredential = 'dockerhub_id'
    }
   stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/RutujaPawal/mypythonrepo.git']]])
            }
        }
         stage('Building image') {
            steps {
                script{
                     dockerImage = docker.build registry
                }
            }
         }
         stage('Uploading Docker images into Docker Hub'){
            steps{
               script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push()
                  }
               }
            } 
         }
         stage('docker stop container'){
             steps{
                   sh 'docker ps -f name=mypythonappContainer -q | xargs --no-run-if-empty docker container stop'
                   sh 'docker container ls -a -fname=mypythonappContainer -q | xargs -r docker container rm'
             }
         }
         stage('Docker Run'){
             steps{
                script {
                   dockerImage.run("-p 8096:5000 --rm --name mypythonappContainer")
                }
             }
         }
    }    
 }   
