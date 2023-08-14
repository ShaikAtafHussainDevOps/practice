pipeline{
    agent any
    stages{
        stage("git cloning"){
            steps{ 
                echo "Cloning the project from github"
                git url:"https://github.com/ShaikAtafHussainDevOps/practice.git", branch:"master"            }
        }
        stage("Build"){
            steps{ 
                echo "Building the code"
                sh 'docker build -t notesappimg:v1 .'
            }
        }
         stage("Push to docker hub"){
            steps{ 
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notesappimg:v1 ${env.dockerHubUser}/notesappimg:v1"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notesappimg:v1"
                }
            }
        }
        stage("Deploy"){
            steps{ 
                echo "Deploying the code"
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
