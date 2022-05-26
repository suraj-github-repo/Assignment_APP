pipeline {
     agent any
        environment {
        //once you sign up for Docker hub, use that user_id here
        registry = "meshrams063/assignmet_app"
        //- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    stages {

        stage ('checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://gitlab.com/suraj-github-repo/assignment_app.git']]])
            }
        }
       
        stage ('Build docker image') {
            steps {
                script {
                dockerImage = docker.build registry
                }
            }
        }
       
         // Uploading Docker images into Docker Hub
    stage('Upload Image') {
     steps{   
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            }
        }
      }
    }
   
    stage ('K8S Deploy') {
        steps {
            script {
                kubernetesDeploy(
                    configs: 'kubernetes-deploy.yml',
                    kubeconfigId: 'K8S',
                    enableConfigSubstitution: true
                    )           
               
            }
        }
    }
  
    }  
}
