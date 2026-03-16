pipeline {
    agent any

    stages {

        stage('Checkout Source Code') {
            steps {
                git branch: 'main', url: 'https://github.com/nakul-krishnakumar/ci-cd-docker-lab.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t 2023bcs0010nakul/bcs10-ci-cd-docker-lab:latest .'
            }
        }

        stage('Test Application') {
            steps {
                echo 'No automated tests required for this static web application'
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                echo 'Logging into DockerHub and pushing image...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        export DOCKER_CONFIG=$WORKSPACE/docker-temp
                        mkdir -p $DOCKER_CONFIG

                        # Build base64 auth token and write directly — no docker login needed
                        AUTH=$(echo -n "$DOCKER_USER:$DOCKER_PASS" | base64 -w 0)
                        echo "{\"auths\":{\"https://index.docker.io/v1/\":{\"auth\":\"$AUTH\"}}}" > $DOCKER_CONFIG/config.json

                        docker --config $DOCKER_CONFIG tag 2023bcs0010nakul/bcs10-ci-cd-docker-lab:latest $DOCKER_USER/bcs10-ci-cd-docker-lab:latest
                        docker --config $DOCKER_CONFIG push $DOCKER_USER/bcs10-ci-cd-docker-lab:latest
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'CI/CD Pipeline completed'
        }
    }
}
