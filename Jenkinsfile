@Library("shared") _
pipeline {
    agent any

    stages {
        stage('hello') {
            steps {
                script {
                    hello()
                }
            }
        }

        stage('scm checkout') {
            steps {
                echo 'This is cloning code'
                git url: "https://github.com/NAYANZAGADE/django-notes-app.git", branch: "main"
            }
        }

stage('Build') {
  steps {
    script {
      docker_build("notes-app", "v1.0", "nayanzagade7")

    }
  }
}

        stage('test') {
            steps {
                echo 'This is testing the code'
                // Add real tests if available
            }
        }

        stage('push to dockerhub') {
            steps {
                script {
                    docker_push("notes-app", "latest", "nayanzagade7")
                }
            }
        }

        stage('deploy') {
            steps {
                echo 'This is deploying the code'
                sh '''
                    echo "Stopping any container on port 8000..."
                    docker ps --filter "publish=8000" --format "{{.ID}}" | xargs -r docker stop || true

                    echo "Removing existing container (if any)..."
                    docker rm -f notes-app || true

                    echo "Running new container..."
                    docker run -itd --name notes-app -p 8000:8000 notes-app:latest
                '''
            }
        }
    }
}

