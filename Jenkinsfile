pipeline {
    agent any
    environment {
        VERSION = 'v1.19.0'
    }
    stages {
        stage('Build and push image to Harbor') {
            steps {
                script {
                    docker.withRegistry('https://harbor.e-btlnet.local', 'harbor') {
                        def image = docker.build("harbor.e-btlnet.local/ecr/healthchecks:${env.VERSION}")
                        image.push()
                    }
                }
            }
        }
        stage('Clean unused docker image') {
            steps{
                sh "docker rmi harbor.e-btlnet.local/ecr/healthchecks:${env.VERSION}"
            }
        }
    }
}
