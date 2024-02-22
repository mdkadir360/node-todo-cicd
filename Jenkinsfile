pipeline {
    agent {
        label 'mynode'
    }
    stages {
        stage('code') {
            steps {
                git url: 'https://github.com/mdkadir360/node-todo-cicd.git', branch: 'master'
            }
        }
        stage('build and test') {
            steps {
                script {
                    // Add a tag for the Docker image
                    sh 'docker build -t mdkadir360/node-todo-app-cicd:latesst .'
                }
            }
        }
         stage('login and push to docker hub') {
            steps {
                script {
                   echo 'loging in to doker hub and pushing the image...'
                  withCredentials([usernamePassword(credentialsId:'docker',passwordVariable:'dockerpassword',usernameVariable:'dockerUsername')]){
                      sh "docker login -u ${env.dockerUsername} -p ${env.dockerpassword} "
                      sh "docker push  mdkadir360/node-todo-app-cicd:latesst"
                  }
                    
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    // Correct the typo in 'docker-compose up'
                    sh 'docker-compose down && docker-compose up -d'
                }
            }
        }
    }
}
