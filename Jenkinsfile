pipeline{
    agent any
    stages{
        stage("Maven Build"){
           steps{
               sh 'mvn package'
           } 
        }
        stage("upload artifact"){
            steps{ 
                nexusArtifactUploader artifacts: [[artifactId: 'doctor-online', 
                                                   classifier: '', file: 'target/doctor-online.war', 
                                                   type: 'war']], 
                                                   credentialsId: 'nexus3', 
                                                   groupId: 'in.javahome', 
                                                   nexusUrl: '172.31.36.80:8081',
                                                   nexusVersion: 'nexus3',
                                                   protocol: 'http', 
                                                   repository: 'do-snapshot', 
                                                   version: '1.3-SNAPSHOT'
            }
        } 
        stage("Dev Deploy"){
           steps{
              sshagent(['tomcat-dev']) {
                // Copy war file to tomcat dev server
                sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@172.31.93.234:/opt/tomcat-9/webapps/"
                // Restart tomcat server
                sh "ssh ec2-user@172.31.93.234 /opt/tomcat-9/bin/shutdown.sh"
                sh "ssh ec2-user@172.31.93.234 /opt/tomcat-9/bin/startup.sh"
              }
           } 
        }
    }
}
