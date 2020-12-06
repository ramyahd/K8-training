@Library('lib')_

 

pipeline {
environment {
registry = "ramyahd97/python"
registryCredential = 'docker'
dockerImage = ''
}
agent any
stages{
stage('Checkout'){
steps{
github_checkout("main", "https://github.com/ramyahd/K8-training.git")
}
}

stage('Initialize'){
steps {
script {
def dockerHome = tool 'aye_Docker'
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
