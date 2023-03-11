pipeline {
    agent any
    stages{
        steps{
            stage('Clone repository') {
                checkout scm
            }
        }

        stage('Build image') {
            steps{
                script{
                    def dockerImage = docker.build("mahshaban95/autograder-jenkins")
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerImage.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }

        
        stage('Trigger ManifestUpdate') {
            steps {
                script{
                    echo "triggering updatemanifestjob"
                    build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
                }
            }       
        }
    }
}