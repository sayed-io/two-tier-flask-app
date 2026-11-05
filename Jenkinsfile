
pipeline {
    agent {label "dev"}

    stages {
        stage("Code Clone") {
            steps {
                git url: 'https://github.com/sayed-io/two-tier-flask-app.git',
                    branch: 'master'

                echo "Code cloning completed"
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
                echo "Docker build completed"
            }
        }

        stage("Test") {
            steps {
                echo "Test completed"
            }
        }

        stage("Push to DockerHub") {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerHubCreds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login \
                            -u "$DOCKER_USER" --password-stdin

                        docker tag two-tier-flask-app \
                            "$DOCKER_USER/two-tier-flask-app:latest"

                        docker push "$DOCKER_USER/two-tier-flask-app:latest"

                        docker logout
                    '''
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d"
                echo "Docker Compose deployment completed"
            }
        }
    }
}
