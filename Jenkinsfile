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
                // Build new Image
                script {
                    gitCommit = sh (script: 'git rev-parse --short HEAD | tr "\n" " "', returnStdout: true)
                    def imageReact = docker.build("${imageName}:${gitCommit}", "-f ${frontedDockerfilePath}/${dockerfile} ${frontedDockerfilePath}")
                }

                // Test if it created

                script {
                    isImageCreated = sh (script: 'docker image ls | grep ${imageName}:${gitCommit}', returnStdout: true)
                    echo isImageCreated
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