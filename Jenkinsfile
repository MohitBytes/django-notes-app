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
        sh '''
            set -x

            echo "Stopping old containers..."
            docker ps -q --filter "publish=8000" | xargs -r docker stop || true
            docker ps -aq --filter "publish=8000" | xargs -r docker rm || true

            echo "Bringing down compose stack..."
            docker compose down || true

            echo "Starting new containers..."
            timeout 120 docker compose up -d

            echo "Deployment status:"
            docker ps
        '''
    }
}
    }
}
