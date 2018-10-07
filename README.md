# new

node {
       stage ('SCM Checkout') {
         git 'https://github.com/sudhakarmuthyala/Pipeline.git'
      }
 
       stage ('Sleep')  {
          sleep 5 
       }
	   
	   stage ('mvn Test'){
           def mvnHome = tool name: 'M2', type: 'maven'
           def mvnCMD = "${mvnHome}/bin/mvn"
           sh "${mvnCMD} test"
      }
          
       stage ('mvn package'){
           def mvnHome = tool name: 'M2', type: 'maven'
           def mvnCMD = "${mvnHome}/bin/mvn"
           sh "${mvnCMD} clean package"
      }
      
       stage("build & SonarQube analysis") {
	   //https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Jenkins#AnalyzingwithSonarQubeScannerforJenkins-Pausepipelineuntilqualitygateiscomputed
              withSonarQubeEnv('sonar') {
                 sh 'mvn clean package sonar:sonar'
              }    
              } 
	  
       stage('nexus repo'){
                     nexusArtifactUploader artifacts: [[artifactId: 'CounterWebapp', classifier: '', file: 'target/CounterWebApp.war', type: 'war']], credentialsId: 'Nexus_cre', groupId: 'msreddy', nexusUrl: '192.168.1.10:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'nexus_pipeline', version: '3.0'

                   }
		  
	   
    }
	
	===================
	
	stage('nexus repo'){
        nexusArtifactUploader artifacts: [[artifactId: 'CounterWebApp', classifier: '', file: 'target/CounterWebApp.war', type: 'war']], credentialsId: 'Nexus_cre', groupId: 'com.mkyong', nexusUrl: '192.168.1.10:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'nexus_pipeline', version: '3.0'
                   }
				   
============================   

		
