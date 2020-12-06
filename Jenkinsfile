
pipeline {
environment {
registry = "ramyahd97/appnode"
registryCredential = 'docker'
dockerImage = ''
}
agent any
stages{
stage('Checkout'){
steps{
checkout([$class: 'GitSCM', branches: [[name: '*/main']],

     userRemoteConfigs: [[url: 'https://github.com/ramyahd/K8-training.git']]])
}
}

stage('Initialize'){
steps {
script {
def dockerHome = tool 'myDocker'
env.PATH = "${dockerHome}/bin:${env.PATH}"
}
}
}

stage('Building our image')
{
steps
{
sh 'docker --version'
script
{
dockerImage = docker.build registry + ":$BUILD_NUMBER"
}
}
}
stage('Deploy image') {
steps {
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.push()
}
}
}
}
// stage('Cleaning up') {
// steps {
// sh "docker rmi $registry:$BUILD_NUMBER"
// }
// }
}
}
