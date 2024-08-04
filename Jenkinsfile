pipeline {
    agent any
    
    stages {
        stage("Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/sumanhaldar01/django-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the image"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image"
                withCredentials([usernamePassword(credentialsId: "DockerHub", passwordVariable: "DockerHubPass", usernameVariable: "DockerHubUser")]) {
                    sh "docker tag notes-app ${env.DockerHubUser}/notes-app:latest"
                    sh "echo ${DockerHubPass} | docker login -u ${DockerHubUser} --password-stdin"
                    sh "docker push ${DockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh "docker run -d -p 4000:4000 sonuh/notes-app"
            }
        }
    }
}
