
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

     
      sh  'docker images'

         }

     }
      stage('Run Image'){
           steps{
               sh ''' 
               if [ $(docker ps -qf "name=app_node") ]
                then
                echo "from if block"
                docker kill app_node && docker rm app_node
                docker run -d -p 8765:8080 --name app_node "${registry}":"${BUILD_NUMBER}"
                docker ps
               else
                echo "from else block"
                docker run -d -p 8765:8080 --name app_node "${registry}":"${BUILD_NUMBER}"
                docker ps
                fi
               '''
           }
       }   
// stage('Cleaning up') {
// steps {
// sh "docker rmi $registry:$BUILD_NUMBER"
// }
// }
}
}
