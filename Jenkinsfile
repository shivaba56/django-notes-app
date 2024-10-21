@Library("Shared") _
pipeline {
    agent {label "node"}

    stages {
        stage("CodeClone") {
            steps {
                script{
                clone("https://github.com/shivaba56/django-notes-app.git","main")
                }
            }
        }
        stage("Build") {
            steps {
               echo "This is for building the code" 
               sh ("whoami")
               sh ("docker build -t notes-app:latest .")
            }
        }
        stage("Push docker image") {
            steps {
               withCredentials([usernamePassword(credentialsId:"dockerHubCred",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
               echo "Pushing docker image"
               sh("docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}")
               sh("docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest")
               sh("docker push ${env.dockerHubUser}/notes-app:latest")
               }
            }
        }
        stage("Deploy") {
            steps {
                echo "This is deploying the code"
                sh ("docker compose up -d")
            }
        }
        
    }
}
