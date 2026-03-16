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

                        # Use Python to safely build config.json — avoids shell quoting issues
                        python3 -c "
        import base64, json, os
        auth = base64.b64encode((os.environ['DOCKER_USER'] + ':' + os.environ['DOCKER_PASS']).encode()).decode()
        config = {'auths': {'https://index.docker.io/v1/': {'auth': auth}}}
        with open(os.environ['DOCKER_CONFIG'] + '/config.json', 'w') as f:
            json.dump(config, f)
        "

                        docker --config $DOCKER_CONFIG push 2023bcs0010nakul/bcs10-ci-cd-docker-lab:latest
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
