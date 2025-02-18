pipeline{

 agent any
 environment{
	PATH="/opt/maven/bin:$PATH"
	}
	stages{
	  stage('Build'){
		steps{
		  sh 'mvn clean install'
		}
	    }
	  stage('sonarQube analysis'){
	    environment{
		scannerHome=tool 'bharath-sonarqube-scanner'
		}
		steps{
		  withSonarQubeEnv('Bharath-SonarQube-Server'){
			sh "${scannerHome}/bin/sonar-scanner"
			}
		    }
		}
	  stage("Quality Gate"){
		  steps{
			  script{
				  timeout(time:1,unit:'hours'){
					  def qg=waitForQualityGate()
					  if (qg.status != 'OK'){
						  echo " warning pipeline aborted due to quality gate failure: ${qg.status}"
					  }
				  }
			  } 
		  } 
	  } 
	}
     }


