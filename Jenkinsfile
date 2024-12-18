pipeline{
    agent { label 'dev-server' }
    
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/AbulFaizBangi/node-todo-cicd.git", branch: "master"
                 echo "Code Cloning Success"
            }
        }
        stage("Code Build & Test"){
            steps{
                sh "docker build -t node-app ."
                echo "Code Build Success"
            }
        }
        stage("Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                sh "docker image tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                sh "docker push ${env.dockerHubUser}/node-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
