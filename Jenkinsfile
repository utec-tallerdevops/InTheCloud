pipeline {
  
    agent none

    stages {
      
        stage('Login en Master') {
         agent {
                node {
                    label 'master'
                }
            }
            steps {
                echo 'Ejecutando comando docker login devopsutec.azurecr.io -u devopsutec -p eYUZB+kRWGt19mj8Lj1RpbmSKnwdtw33...'
                sh 'docker login devopsutec.azurecr.io -u devopsutec -p eYUZB+kRWGt19mj8Lj1RpbmSKnwdtw33'
           }
        }
        
        stage('Construcción de Imagenes en agente Master') {
            agent {
                node {
                    label 'master'


                }
            }
            steps {
                echo 'Construcción de Imagenes en agente Master...'
              
                  dir('worker'){ 
                    sh 'docker build -t devopsutec.azurecr.io/inthecloud-worker-1.0:${BUILD_NUMBER} .'
                  }
              
                dir('vote'){ 
                    sh 'docker build -t devopsutec.azurecr.io/inthecloud-vote-1.0:${BUILD_NUMBER} .'
                  }
              
                dir('result'){ 
                    sh 'docker build -t devopsutec.azurecr.io/inthecloud-result-1.0:${BUILD_NUMBER} .'
                  }
            }
        }

        stage('Push de Imagenes en agente Master') {
            agent {
                node {
                    label 'master'


                }
            }
            steps {
                    echo 'Push de Imagenes en agente Master...'
                    withDockerRegistry(credentialsId: 'inthecloud', url:'https://devopsutec.azurecr.io'){ 
                        sh 'docker push devopsutec.azurecr.io/inthecloud-worker-1.0:${BUILD_NUMBER}'
                        sh 'docker push devopsutec.azurecr.io/inthecloud-vote-1.0:${BUILD_NUMBER}'
                        sh 'docker push devopsutec.azurecr.io/inthecloud-result-1.0:${BUILD_NUMBER}'
                    }
                    
            }
        }


         stage('Deploy con docker-compose en agente UTEC') {
         agent {
                node {
                    label 'utec'
                }
            }
            steps {
            withDockerRegistry(credentialsId: 'inthecloud', url:'https://devopsutec.azurecr.io'){ 
                echo 'Verificacion previa a realizar docker-compose...'
                echo 'Ejecutando comando docker ps...'
                sh 'docker ps'
                echo 'Ejecutando deploy con docker-compose up -d...'
                sh 'docker-compose up -d'
                echo 'Verificacion posterior a docker-compose...'
                echo 'Ejecutando comando docker ps...'
                sh 'docker ps'
                }
           }
        }

      
    }

        }
