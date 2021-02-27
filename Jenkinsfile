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
                    gitCommit = sh (script: 'git rev-parse --short HEAD | tr "\n" " "', returnStdout: true)
                    def imageReact = docker.build("${imageName}:${gitCommit}", "-f ${dockerfile} ${frontedDockerfilePath}")
                    //sh "docker build -t ${imageName}:${gitCommit} -f ${dockerfile} ${frontedDockerfilePath}"
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