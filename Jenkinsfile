pipeline {
    agent any
    stages {
        stage('Cloning') {
            steps {
                echo "Cloning the Repo"
                git url: "https://github.com/LondheShubham153/node-todo-cicd.git", branch: "master"
            }
        }
        stage('Building') {
            steps {
                echo "Building the Repo"
                sh "docker build . -t node-todo-cicd"
            }
        }
        stage('Pushing to DockerHub') {
            steps {
                echo "Pushing to DockerHub"
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                    sh "docker tag node-todo-cicd ${env.DOCKERHUB_USERNAME}/node-todo-cicd:latest"
                    sh "docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD"
                    sh "docker push ${env.DOCKERHUB_USERNAME}/node-todo-cicd:latest"
                }
            }
        }
        stage('Deploying') {
            steps {
                echo "Deploying to host"
                sh "docker-compose down"
                sh "docker-compose up -d"
            }
        }
    }
}

// teslaop/node-todo-cicd:latest
