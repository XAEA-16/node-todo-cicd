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
                withCredentials([usernamePassword(credentialsId: '  ', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                    sh "docker tag node-todo-cicd ${env.DOCKERHUB_USERNAME}/node-todo-cicd:latest"
                    sh "docker login -u ${env.DOCKERHUB_USERNAME} -p ${env.DOCKERHUB_PASSWORD}"
                    sh "docker push ${env.DOCKERHUB_USERNAME}/node-todo-cicd:latest"
                }
            }
        }
        stage('Deploying') {
            steps {
                echo "Deploying to host"
                sh "docker run -d DOCKERHUB_USERNAME/node-todo-cicd:latest"
            }
        }
    }
}
