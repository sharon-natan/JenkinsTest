// Define Variables
def gitCommit = ""
def isImageCreated = false
def dockerfile = "Dockerfile"
def imageName = "fronted-react"
def frontedDockerfilePath = "./docker-create-react-app"

pipeline {
    agent any
    stages {
        stage('Create new Image') {
            steps {
                script {
                    gitCommit = sh (script: 'git rev-parse --short HEAD', returnStdout: true)
                    def imageReact = docker.build("${imageName}:${gitCommit}", "${frontedDockerfilePath} .")
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