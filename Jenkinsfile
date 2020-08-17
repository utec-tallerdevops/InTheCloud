pipeline {
  
    agent none

    stages {
      
        stage('Login') {
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
        
        stage('Construcción/Compilación de Imagenes en Master') {
            agent {
                node {
                    label 'master'


                }
            }
            steps {
                    echo 'Construcción/Compilación de Imagenes en Master...'
              
                  dir('worker'){ 
                    sh 'docker build -t devopsutec.azurecr.io/inthecloud-worker-1.0.0'
                  }
              
                dir('vote'){ 
                    sh 'docker build -t devopsutec.azurecr.io/inthecloud-vote-1.0.0'
                  }
              
                dir('result'){ 
                    sh 'docker build -t devopsutec.azurecr.io/inthecloud-result-1.0.0'
                  }
            }
        }

        stage('Push de Imagenes en Master') {
            agent {
                node {
                    label 'master'


                }
            }
            steps {
                    echo 'Push de Imagenes en Master...'
                    withDockerRegistry(credentialsId: 'InTheCloud', url:'https://devopsutec.azurecr.io'){ 
                        sh 'docker push devopsutec.azurecr.io/inthecloud-worker-1.0.0'
                        sh 'docker push devopsutec.azurecr.io/inthecloud-vote-1.0.0'
                        sh 'docker push devopsutec.azurecr.io/inthecloud-result-1.0.0'
                    }
                    
            }
        }
/*
         stage('Verificacion') {
         agent {
                node {
                    label 'utec'
                }
            }
            steps {
                echo 'Ejecutando comando docker ps...'
                sh 'docker ps'
           }
        }
    */

         stage('Deploy con docker-compose') {
         agent {
                node {
                    label 'utec'
                }
            }
            steps {
            withDockerRegistry(credentialsId: 'InTheCloud', url:'https://devopsutec.azurecr.io'){ 
                echo 'Verificacion Pre docker-compose...'
                echo 'Ejecutando comando docker ps...'
                sh 'docker ps'
                echo 'Ejecutando deploy con docker-compose up -d...'
                sh 'docker-compose up -d'
                echo 'Verificacion Post docker-compose...'
                echo 'Ejecutando comando docker ps...'
                sh 'docker ps'
                echo 'Ejecutando comando docker images...'
                sh 'docker images'
                }
           }
        }

       /*
         stage('Docker Compose Down') {
         agent {
                node {
                    label 'utec'
                }
            }
            steps {
                echo 'Ejecutando comando docker-compose down...'
                sh 'docker-compose down'
           }
        }
        stage('Verificacion Final') {
         agent {
                node {
                    label 'utec'
                }
            }
            steps {
                echo 'Ejecutando comando docker ps...'
                sh 'docker ps'
                echo 'Ejecutando comando docker images...'
                sh 'docker images'
           }
        }
       stage('Logout') {
         agent {
                node {
                    label 'utec'
                }
            }
            steps {
                echo 'Ejecutando comando docker logout...'
                sh 'docker logout'
           }
        }
        */
    }

        }
