pipeline {
    agent{      
    node { label 'myjenkinsdocker'}     
  } 
environment{
dockerhub=credentials('dockerhub')
}
   tools
    {
       maven "mymaven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/knbtechnosys/mydeploy.git'
             
          }
        }
  stage('Execute Maven') {
           steps {
                             sh 'mvn package'             
          }
        }

 stage('Build Docker Image') {         
      steps{                
	sh 'sudo docker build -t nbktechnosys/myjenkinsdocker:$BUILD_NUMBER .'           
        echo 'Build Image Completed'                
      }           
    }

    stage('Login to Docker Hub') {         
      steps{  
	sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'      
	echo 'Login Completed'                
      }           
    }               
    stage('Push Image to Docker Hub') {         
      steps{                            
	sh 'sudo docker push nbktechnosys/myjenkinsdocker:$BUILD_NUMBER'                
	 echo 'Push Image Completed'       
      }           
    }      
  
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
   {
                sh "docker run -d -p 8003:8080 nbktechnosys/myjenkinsdocker:$BUILD_NUMBER"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@172.31.46.251 run -d -p 8003:8080 nbktechnosys/myjenkinsdocker:$BUILD_NUMBER"
 
            }
        }
    }
 }
