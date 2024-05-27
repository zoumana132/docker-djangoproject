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
                withCredentials([usernamePassword(credentialsId: 'docker_hub_cred', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                    sh '''
                    docker tag django-todo-app $DOCKER_HUB_USERNAME/django-todo-app:latest
                    echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD --password-stdin
                    docker push $DOCKER_HUB_USERNAME/django-todo-app:latest
                    '''
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
