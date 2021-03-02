// Define Variables
def gitCommit = ""
def isImageCreated = false
def isContainerUp = false
def dockerfile = "Dockerfile"
def imageName = "frontend-react"
def frontendDockerfilePath = "./docker-create-react-app"
def containerName = ["frontend": "react_container", "backend": "backend_container"]
def containerIP = ["frontend": "", "backend": ""]

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
                    def imageReact = docker.build("${imageName}:${gitCommit}", "-f ${frontendDockerfilePath}/${dockerfile} ${frontendDockerfilePath}")
                }

                // Test if it created

                script {
                    imageInfo = sh (script: "docker images ${imageName}", returnStdout: true)
                    isImageCreated = imageInfo.contains(imageName)
                    
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
                    //def conainer = docker.image("${imageName}:${gitCommit}").withRun('-p 3000:3000')
                    sh "docker run -d -p 3000:3000 --name ${containerName["frontend"]} ${imageName}:${gitCommit}"
                    runningContainers = sh (script: 'docker ps', returnStdout: true)
                    isContainerUp = runningContainers.contains(containerName["frontend"])
                    containerIP["frontend"] = sh (script: "docker inspect --format '{{ .NetworkSettings.IPAddress }}' ${containerName["frontend"]} | tr -d ' ' | tr '\n' ' '", returnStdout:true)
                    echo containerIP["frontend"]

                    if (!isContainerUp){
                        currentBuild.result = 'ABORTED'
                        error('The container could not start.')
                    }
                }
            }
        }

        stage('Testing') {
            steps{
                script {
                      def response = httpRequest "http://${containerIP["frontend"]}:3000"
                      println("Status: "+response.status)
                      println("Content: "+response.content)
                }
            }
        }
    }

    post {
        always {
            script {
                if(isContainerUp) {
                    sh "docker rm ${containerName["frontend"]} -f"
                }
                if(isImageCreated){
                    sh "docker image rm ${imageName}:${gitCommit} --force"
                }
            }
        }
    }
}