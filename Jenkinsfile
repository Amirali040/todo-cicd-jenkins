pipeline{
    agent { label 'dev' }
    
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/Amirali040/todo-cicd-jenkins.git", branch: "master"
            }
        }
        stage("Code Build & Test"){
            steps{
                echo "Code Build Stage"
                sh "docker image inspect todo-cicd:latest > /dev/null 2>&1 || docker build -t todo-cicd ."
            }
        }
        stage("Image Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubPass",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                sh "docker image tag todo-cicd:latest ${env.dockerHubUser}/todo-cicd:latest"
                sh "docker push ${env.dockerHubUser}/todo-cicd:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d --build"
            }
        }
    }
}
