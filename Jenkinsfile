pipeline {
  agent any
tools {
  maven 'M2_HOME'
  jdk 'JAVA_HOME'
}

    stages {

      stage ('Checkout SCM'){
        steps {
          checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/raghebbenhamouda/hello-world.git']]])
        }
      }
	  
	  stage ('Build')  {
	      steps {
          
            sh "mvn clean install "
          
        }
	  }

	  stage ('copy artifact to ansible')  {
	      steps {
          
           sshagent(['deployer_user']) {
    
            sh"  scp -o StrictHostKeyChecking=no webapp/target/webapp.war ec2-user@18.219.43.144:/opt/kubernetes"


                 }
          
        }
         
      }
      
      	  stage ('BUild & Push of Docker image')  {
	      steps {
          
           sshagent(['deployer_user']) {
    
            sh "ssh -o StrictHostKeyChecking=no ec2-user@18.219.43.144 -C \"sudo ansible-playbook -i /opt/kubernetes/hosts /opt/kubernetes/create-simple-devops-image.yml\""


                                       }
          
                 }
        
         
              }
              
      	  stage ('Deploy to Kubernetes')  {
	      steps {
          
           sshagent(['deployer_user']) {
    
            sh "ssh -o StrictHostKeyChecking=no ec2-user@18.219.43.144 -C \"ansible-playbook -i /opt/kubernetes/hosts /opt/kubernetes/deployment.yml\""


                                       }
          
                 }
        
         
              }              
      
      
   }
  
}
