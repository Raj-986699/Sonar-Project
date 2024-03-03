pipeline{
    agent any
    environment {
        PATH = "$PATH:/opt/apache-maven-3.9.6/bin"
    }
    stages{
       stage('Checkout'){
            steps{
                git 'git@github.com:NagiReddyDEVOPS/Sonar-Project.git'
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
        sh "mvn clean verify sonar:sonar -Dsonar.projectKey=sonar-project -Dsonar.host.url=http://13.127.158.230:9000 -Dsonar.login=sqp_c1520b88484f0943a1a85880acbbbcb0b79d469e"
    }
        }
        }
       
    stage ('Deploy to Nexus') {
            steps {
      nexusArtifactUploader(
      nexusVersion: 'nexus3',
      protocol: 'http',
      nexusUrl: '43.204.221.160:8081',
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
        sh 'scp -o StrictHostKeyChecking=no webapp/target/webapp.war root@3.7.46.241:/opt/apache-tomcat-8.0.52/webapps'
           }
   }
}    
}
post {
        success {
            emailext (
                subject: "Pipeline Success: ${env.JOB_NAME}",
                body: "The pipeline ${env.JOB_NAME} has successfully completed.",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                to: "manchala.ksvc@gmail.com",
                replyTo: "manchala.ksvc@gmail.com",
                from: "manchala.ksvc@gmail.com",
                attachLog: true,
                compressLog: true
            )
        }
        failure {
            emailext (
                subject: "Pipeline Failure: ${env.JOB_NAME}",
                body: "The pipeline ${env.JOB_NAME} has failed. Please check the Jenkins console output for more details.",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                to: "manchala.ksvc@gmail.com",
                replyTo: "manchala.ksvc@gmail.com",
                from: "manchala.ksvc@gmail.com",
                attachLog: true,
                compressLog: true
            )
        }
    }
