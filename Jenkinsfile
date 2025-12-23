@Library("Shared") _
pipeline {
    agent { label "vinod" }

    stages {
        stage("Hello") {
            steps{
                script{
                    hello()
                }
            }
        }
        stage("Code") {
            steps {
                script{
                    clone("https://github.com/MohitBytes/django-notes-app.git","main")
                }
            }
        }

        stage("Build") {
            steps {
                script{
                docker_build("notes-app","latest","mohitparmar23")
                }
            }
        }

        stage("Push to DockerHub") {
            steps {
                script{
                    docker_push("notes-app","latest","mohitparmar23")
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying application"
                sh """
                    docker ps -q --filter "publish=8000" | xargs -r docker stop
                    docker ps -aq --filter "publish=8000" | xargs -r docker rm
                    docker compose up -d
                """
            }
        }
    }
}
