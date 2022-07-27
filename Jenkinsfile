pipeline {
    agent any
    
    environment {
    registry = "sveggalam/trail"
    registryCredential = 'docker_credentials'
    dockerImage = ''
  }
    tools {
        //tools that are going to be used 
        nodejs 'nodejs'
        dockerTool 'docker'
    }

    stages {
        stage('Git') {
            steps {
                echo 'cloning github repo'
                git 'https://github.com/sveggalam07/Node.js-Hello-World-Microservice-Example.git'
            }
        }
        stage('Build')
        {
            steps{
                  echo 'dependencies installing'
                  sh "npm install"
            }
        }
        stage('Test'){
            steps{
                echo 'tests running'
                sh 'npm test'
            }
        }
        stage('Image'){
            steps{
                script {
                    // Problem 1
                    //Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
                    // started docker daemon
                    // Problem 2
                    // Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: 
                    // added jenkins user to docker
                    echo 'Building docker image '
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    
            }
            }
        }
       stage('Deploy Image') {
      steps{
         script {
            echo 'Deploying docker image to dockerhub'
            docker.withRegistry('https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("${env.BUILD_NUMBER}")            
          }
        }
      }
    }
    }
}
