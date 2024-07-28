pipeline {
    
    agent {
        node {
            label 'dev'
        }
    }

    stages {
        
        stage('Clone Code') {
            steps {
                git url: "https://github.com/rajatchauhan-git/django-notes-app/", branch: "main"
                echo 'Code clone done'
            }
        }
        
        stage('Build & Test') {
            steps {
                sh "whoami"
                sh "docker build -t notes-app-jenkins:latest ."
                echo 'Docker build done'
            }
        }
        
        stage ('Push to Docker Hub') {
            steps {
                withCredentials(
                    [usernamePassword(
                        credentialsId:"DockerCreds",
                        passwordVariable:"dockerHubPass", 
                        usernameVariable:"dockerHubUser"
                        )
                    ]
                ){
                sh "docker image tag notes-app-jenkins:latest ${env.dockerHubUSer}/note-app-jenkins:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUSer}/note-app-jenkins:latest"
                echo 'Pushed to docker hub'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh "docker compose up -d"
                echo 'Docker deploy done'
            }
        }
    }
}
