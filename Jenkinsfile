pipeline {
    agent any
    def dockerImage

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       dockerImage = docker.build("mahshaban95/autograder-jenkins")
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            dockerImage.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
            echo "triggering updatemanifestjob"
            build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}   