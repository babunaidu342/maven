pipeline{
    agent any
    stages{
        stage("git checkout"){
            steps{
                git credentialsId: 'github-creds', url: 'https://github.com/babunaidu342/maven'
            }
        }       
        stage("maven build"){
            steps{
                sh 'mvn clean package' 
            }
        }
        stage("dev tomcat deploy"){
            steps{
                sshagent(['tomcat-dev']) {
                  sh "mv target/myweb*.war target/app.war"
                  //copy war
                  sh "scp -o StrictHostKeyChecking=no target/app.war ec2-user@172.31.85.218:/opt/tomcat9/webapps"
                  // stop tomcat
                  sh "ssh ec2-user@172.31.85.218 /opt/tomcat9/bin/shutdown.sh"
                  // start tomcat
                  sh "ssh ec2-user@172.31.85.218 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
    }
}
