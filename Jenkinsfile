// Define Variables
def gitCommit = ""
def isImageCreated = false
def isContainerUp = false
def dockerfile = "Dockerfile"
def imageName = "fronted-react"
def frontedDockerfilePath = "./docker-create-react-app"
def containerName = ["fronted": "react_container", "backend": "backend_container"]

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
                    imageInfo = sh (script: "docker images ${imageName}", returnStdout: true)
                    isImageCreated = imageInfo.contain(imageName)
                    
                    if (!isImageCreated){
                        currentBuild.result = 'ABORTED'
                        error('Image could not be built.')
                    }
                }
            }
        }
        stage('Start Container') {
            steps {
                script {
                    def conainer = docker.image("${imageName}:${gitCommit}").withRun("-p 3000:3000 --name ${containerName["fronted"]}")
                    runningContainers = sh 'docker ps'
                    isContainerUp = runningContainers.contain({containerName["fronted"]})
                }
            }
        }
    }

    post {
        always {
            if(isContainerUp) {
                sh "docker kill ${containerName["fronted"]}"
            }
            if(isImageCreated){
                sh "docker image rm ${imageName}:${gitCommit} --force"
            }
        }
    }
}