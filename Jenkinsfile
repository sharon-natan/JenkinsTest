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
                    def imageReact = docker.build("react:${gitCommit} -f ${dockerfile} ./docker-create-react-app/")
                    //def customImage = docker.build("my-image:${gitCommit} -f ${dockerfile} ./docker-create-react-app")
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