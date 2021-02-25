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
        stage('Lint') {
            steps {
                script {
                    sh 'hadolint --format json Dockerfile > hadolint.json'
                }
            }
        }
    }
    post {
        always {
            recordIssues enabledForFailure: true, tool: hadoLint(pattern: "hadolint.json")
            googlechatnotification url: 'https://chat.googleapis.com/v1/spaces/AAAAWfyWiL4/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=5vi2ACzaX4THK6Jyl8kmhPrNEOBtJB5hJHtfIwmi-78%3D', message: '${PROJECT_NAME} : Build #${BUILD_NUMBER} - ${BUILD_STATUS} - Issues: ${ANALYSIS_ISSUES_COUNT}\n\nCheck output at ${BUILD_URL}', notifyAborted: 'true', notifyFailure: 'true', notifyNotBuilt: 'true', notifySuccess: 'true', notifyUnstable: 'true', notifyBackToNormal: 'true', suppressInfoLoggers: 'true', sameThreadNotification: 'true'
        }
    }
}
