// Define Variables
def gitCommit = ""
def isImageCreated = false
def dockerfile = "Dockerfile"

pipeline {
    agent any
    stages {
        stage('Create new Image') {
            steps {
                script {
                    gitCommit = sh (script: 'git rev-parse --short HEAD', returnStdout: true)
                    def imageReact = docker.build("react:${gitCommit}", "./docker-create-react-app")
                }
            }
        }
    }

    post {
        always {
            echo 'testing'
        }
    }
}