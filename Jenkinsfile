// Define Variables
def gitCommit = ""
def isImageCreated = false
def dockerfile = "Dockerfile"
def imageName = "fronted-react"
def frontedDockerfilePath = "./docker-create-react-app"

pipeline {
    agent any
    stages {
        stage('Config pipeline') {
            steps {
                script {
                    sh 'git config --global user.name "Jenkins CI"'
                    sh 'git config --global user.email jenkinsCI@jenkins.com'
                }
            }    
        }
        stage('Create new Image') {
            steps {
                // Build new Image
                script {
                    gitCommit = sh (script: 'git rev-parse --short HEAD | tr "\n" " "', returnStdout: true)
                    def imageReact = docker.build("${imageName}:${gitCommit}", "-f ${frontedDockerfilePath}/${dockerfile} ${frontedDockerfilePath}")
                }

                // Test if it created

                script {
                    imageInfo = sh (script: "docker image inspect ${imageName}:${gitCommit}", returnStdout: true)
                    if(!imageInfo){
                        isImageCreated = true
                    }

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