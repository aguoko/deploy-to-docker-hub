def remote = [:]
    remote.name = 'Docker'
    remote.host = '13.40.84.226'
    remote.user = 'ubuntu'
    remote.password = 'Muna@11'
    remote.allowAnyHosts = true


pipeline {
  agent any
  environment {
       imagename = "okoou/docker-class"
       registryCredential = 'DockerHub'
       dockerImage = ''
           }
  stages {
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Building Docker image') {
          steps{
                script {
                     dockerImage = docker.build imagename
                          }
                      }
                }
     stage('Push Image to dockerHub') {
           steps{
               script {
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                                              }
                                    }
                             }
                  }
     
      stage('Remote SSH') {
          steps{
      sshCommand remote: remote, command: "ls -lrt"
      sshCommand remote: remote, command: "docker run -d -p 8080:8080 okoou/docker-class:1"
      }
      }
    
     stage('Remove Unused docker image') {
          steps{
              sh "docker rmi $imagename:$BUILD_NUMBER"
              sh "docker rmi $imagename:latest"
                        }
            }
      }
  }
