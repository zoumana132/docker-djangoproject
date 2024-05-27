pipeline {
    agent { label "Jenkins-agent" }
    stages {
        stage("Clone Code") {
            steps {
                git url: "https://github.com/zoumana132/docker-djangoproject.git", branch: "main"
            }
        }
        stage("Build and Test") {
            steps {
                sh "docker build . -t django-todo-app"
            }
        }
        stage("Login and Push image") {
            steps {
                withCredentials([usernamePassword(credentialsId: "docker_hub_cred", passwordVariable: "docker_hub_credPassword", usernameVariable: "docker_hub_credUsername")]) {
                    script {
                        sh """
                        docker tag django-todo-app ${docker_hub_credUsername}/django-todo-app:latest
                        echo ${docker_hub_credPassword} | docker login -u ${docker_hub_credUsername} --password-stdin
                        docker push ${docker_hub_credUsername}/django-todo-app:latest
                        """
                    }
                }
            }
        }
        stage("Deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
