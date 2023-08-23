pipeline{
    agent any
    stages{
        stage("Clone"){
            steps{
                echo "Cloning The Code"
                git url:"https://github.com/Dips-1991/django-notes-app.git", branch:"main"
            }
        }
        stage("Build"){
            steps{
                echo "Building The code"
                sh "docker build -t django-app-image ."
            }    
        }
        stage("Push"){
            steps{
                echo "Pushing The Image To DockerHub"
                sh "docker images"
                withCredentials([usernamePassword(credentialsId:"DockerHubCredentials", passwordVariable:"dockerPassword", usernameVariable:"dockerUsername")])
                {
                sh "docker tag django-app-image ${env.dockerUsername}/django-app-image:v1"
                sh "docker login -u ${env.dockerUsername} -p ${env.dockerPassword}"
                sh "docker push ${env.dockerUsername}/django-app-image:v1"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying The Application"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    
    }
}
