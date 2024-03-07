pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-maven-3.9.6/bin"
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
           post {
               always {
                 junit '***/target/surefire-reports/TEST-*.xml'
         }
         }
         }
       
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('SonarQube') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn clean verify sonar:sonar -Dsonar.projectKey=sonar-project -Dsonar.host.url=http://3.110.204.25:9000 -Dsonar.login=sqp_cdc489969c57123323b72898e01f60463d986899"
    }
        }
        }
       
    stage ('Deploy to Nexus') {
            steps {
      nexusArtifactUploader(
      nexusVersion: 'nexus3',
      protocol: 'http',
      nexusUrl: '43.204.100.145:8081',
      groupId: 'myGroupId',
      version: '1.0-SNAPSHOT',
      repository: 'maven-snapshots',
      credentialsId: nexuscredentials',
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
        sh 'scp -o StrictHostKeyChecking=no webapp/target/webapp.war root@13.232.216.126:/opt/apache-tomcat-8.0.52/webapps'
        
           }
        
    }
}
}
   

