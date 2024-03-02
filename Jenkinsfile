pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-maven-3.9.6/bin"
    }
	  post {
        success {
            emailext(
                to: 'yesuraju8989@gmail.com', // Email address of the recipient
                subject: "Jenkins Pipeline - Build Successful",
                body: "The Jenkins pipeline has completed successfully.",
                mimeType: 'text/html'
            )
        }
        failure {
            emailext(
                to: 'yesuraju8989@gmail',
                subject: "Jenkins Pipeline - Build Failed",
                body: "The Jenkins pipeline has failed. Please check the build logs for details.",
                mimeType: 'text/html'
            )
        }
    }

    stages{
       stage('Checkout'){
            steps{
                git 'git@github.com:Raj-986699/Sonar-Project.git'
            }
         }        
       stage('Package'){
            steps{
                sh 'mvn clean package'
            }
         }    
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('SonarQube') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn clean verify sonar:sonar -Dsonar.projectKey=sonar-project -Dsonar.host.url=http://3.109.123.226:9000/ -Dsonar.login=sqp_c47308a3198dd18bfd8ecb77b2da5b98f9b31b77"
    }
        }
        }
       
    stage ('Deploy to Nexus') {
            steps {
      nexusArtifactUploader(
      nexusVersion: 'nexus3',
      protocol: 'http',
      nexusUrl: '43.204.96.104:8081',
      groupId: 'myGroupId',
      version: '1.0-SNAPSHOT',
      repository: 'maven-snapshots',
      credentialsId: 'nexuscredentials',
      artifacts: [
      [artifactId: 'maven-project',
      classifier: '',
      file: 'webapp/target/webapp.war',
      type: 'war']
      ])
      }
        }
        stage ('Deploy to Prod'){
     steps {
        sh 'scp -o StrictHostKeyChecking=no webapp/target/webapp.war root@13.235.90.204:/opt/apache-tomcat-8.0.52/webapps'
           }
   } 
 
}
}


