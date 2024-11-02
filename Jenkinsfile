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
                    script {
                        sh '''
                            docker tag node-todo-cicd ${DOCKERHUB_USERNAME}/node-todo-cicd:latest
                            echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin
                            docker push ${DOCKERHUB_USERNAME}/node-todo-cicd:latest
                        '''
                    }
                }
            }
        }
        stage('Deploying') {
            steps {
                echo "Deploying to host"
                sh "docker run -d ${DOCKERHUB_USERNAME}/node-todo-cicd:latest"
            }
        }
    }
}
