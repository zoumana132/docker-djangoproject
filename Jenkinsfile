pipeline{
    agent {label "Jenkins-agent"}
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/zoumana132/docker-djangoproject.git", branch: "main"
            }
        }
        stage("Build and Test") {
            steps{
                sh "docker build . -t django-todo-app"
            }
        }
        stage("Login and Push image")
        {
            steps{
                withCredentials([usernamePassword(credentialsId:"docker_hub_cred",passwordVariable:"docker_hub_credPassword",usernameVariable:"docker_hub_credUsername")]){
                    sh """
                    #!/bin/sh
                    docker tag django-todo-app ${env.docker_hub_credUsername}/django-todo-app:latest
                    docker login -u ${env.docker_hub_credUsername} -p ${env.docker_hub_credPassword}
                    docker push ${env.docker_hub_credUsername}/django-todo-app:latest
                    """
                }
            }
        }
        stage("Deploy") {
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
