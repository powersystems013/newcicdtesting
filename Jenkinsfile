pipeline {
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git(url: 'https://github.com/powersystems013/cicdtesting.git', branch: 'main')
      }
    }

    stage('Building image') {
      parallel {
        stage('Building image') {
          steps {
            script {
              dockerImage = docker.build "${imagename}:latest"
            }

          }
        }

        stage('test') {
          steps {
            sh 'echo " testing is completed"'
          }
        }

      }
    }

    stage('Running image') {
      steps {
        script {
          sh "docker run -d --name ${containerName} ${imagename}:latest"
        }

      }
    }

    stage('Stop and Remove Container') {
      steps {
        script {
          sh "docker stop ${containerName} || true"
          sh "docker rm ${containerName} || true"
        }

      }
    }

    stage('Deploy Image') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: dockerHubCredentials, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
            sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"

            // Push the image
            sh "docker push ${imagename}:latest"
          }
        }

      }
    }

  }
  environment {
    imagename = 'kiet019/jenkins1'
    dockerImage = ''
    containerName = 'my-container'
    dockerHubCredentials = 'admin'
  }
}