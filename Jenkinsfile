pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    configFileProvider([configFile(fileId: 'efecb88a-4a30-4256-954b-47ed3f03e5d7', targetLocation: '.env')]) {
                        sh 'docker build -t todo-list-app .'
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]) {
                        sh """
                            docker login -u '${DOCKERHUB_USER}' -p '${DOCKERHUB_PASS}'
                            docker tag todo-list-app ${DOCKERHUB_USER}/todo-list-app:latest
                            docker push ${DOCKERHUB_USER}/todo-list-app:latest
                        """
                    }
                }
            }
        }

        stage('Deploy to Development') {
            steps {
                script {
                    sh """
                        docker rm -f todo-list-dev
                        docker run -d -p 8001:8000 --name todo-list-dev pcmadevops/todo-list-app:latest
                    """
                }
            }
        }

    }
}

