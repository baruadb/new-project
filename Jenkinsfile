pipeline {
    agent any
    tools {
	    maven "MAVEN3.9"
	    jdk "JDK17"
	}

    environment {
        registryCredential = 'ecr:us-east-2:awscreds'
        appRegistry = "281908687347.dkr.ecr.us-east-2.amazonaws.com/vprofileappimg"
        vprofileRegistry = "https://281908687347.dkr.ecr.us-east-2.amazonaws.com"
    }
  stages {
    stage('Fetch code'){
      steps {
        git branch: 'docker', url: 'https://github.com/baruadb/new-project.git'
      }
    }


    stage('Test'){
      steps {
        sh 'mvn test'
      }
    }

    stage('Build App Image') {
       steps {
       
         script {
                dockerImage = docker.build( appRegistry + ":$BUILD_NUMBER", "./Docker-files/app/multistage/")
             }

     }
    
    }

    stage('Upload App Image') {
          steps{
            script {
              docker.withRegistry( vprofileRegistry, registryCredential ) {
                dockerImage.push("$BUILD_NUMBER")
                dockerImage.push('latest')
              }
            }
          }
     }

  }
}
