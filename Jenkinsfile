node {
stage('clone repo') {
           
        git credentialsId: 'GIT-Credentials', url: 'https://github.com/omprakashpandit/maven-web-app-master.git'
        
   }
 stage('JACOCO'){
        jacoco()
     }	
	
 stage ('Maven Clean Build'){
        def mavenHome = tool name: "Maven-3.8.6", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
 stage('SonarQube analysis') {
		//	withSonarQubeEnv('Sonar-Server-9.5') {
		withSonarQubeEnv(credentialsId: 'SONAR9.5-TOKEN') {
    		def mavenHome = tool name: "Maven-3.8.6", type: "maven"
			def mavenCMD = "${mavenHome}/bin/mvn"
			sh "${mavenCMD} sonar:sonar"
    	}
    }
 stage('upload war to nexus'){
      nexusArtifactUploader artifacts: [[artifactId: '01-maven-web-app', classifier: '', file: 'target/01-maven-web-app.war', type: 'war']], credentialsId: 'NEXUS-Credentials', groupId: 'in.jenkin', nexusUrl: '65.1.86.14:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'JenKin-Repository', version: '1.0-SNAPSHOT'
  }
stage ('Deploy into Tomcat'){
      	 sshagent(['Tomcat-Server-Agent']) {
         sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ec2-user@13.233.90.93:/opt/apache-tomcat-10.0.27/webapps'
       }
    }
 
 
}
