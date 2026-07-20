@Library('shared') _

pipeline{
    agent { label "flask"}
    stages {
        stage( "code")
        {
            steps
            {
                script{
                // echo "this is cloning the code"
                gitclone("https://github.com/aliahmadshah56/flask_mysql_connector.git", "master")
                // echo "code clone successfully..."
                
                }
                
            }
        }

        stage( "build")
        {
            steps
            {
                script{
                    docker_build(
                        imageName: "flash-app",
                        imageTag: "latest"
                        )
                }    
            }
            
        }
        stage( "test")
        {
            steps
            {
                echo " this is testing the code"
            }
        }
        stage("push")
        {
            steps
            {
                script{
                    docker_push(
                        imageName: "flask-app",
                        imageTag: "latest",
                        credentials: "dockerhub-cred"
                        )
                }        
            }
        }
        stage( "deploy")
        {
            steps
            {
                echo "this is deploying the code"
                // sh "docker run -d flask-app:latest"
                sh "docker compose down && docker compose up --build"
            }
        }
        
    }
        
}


