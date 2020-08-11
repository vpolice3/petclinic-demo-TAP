pipeline {
  agent any
  stages{
    stage ('Build') {
      steps{
        echo "Building Project"
        sh 'mvn install -DskipTests=true'
      }
    }
    stage ('Archive') {
      steps{
        echo "Archiving Project"
        archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
      }
    }
   stage('Build images') {
	      steps {
		sh '''
			  cd sm-shop
			  docker build -f "Dockerfile" -t vikaspolicedockerhub/shopizer-app:latest . 
		'''
      }
    }
   stage('Push Image'){
	       steps{
	         withDockerRegistry([credentialsId: 'DOCKER_HUB_CREDENTIALS', url: '']) {
   			sh 'docker push vikaspolicedockerhub/shopizer-app:latest'
	}
      }
    }
   stage('Deploy to dev env') {
	      steps {
		sh '''
			  docker rm -f shopizer-app || true
			 docker run -d --name=shopizer-app -p 8081:8080 vikaspolicedockerhub/shopizer-app:latest  
		'''
      }
    }
  }
}
