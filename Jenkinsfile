pipeline {
  agent any
  parameters{
    string(name: 'tomcat_dev', defaultValue: '34.216.112.154', description: 'Staging Server')
    string(name: 'tomcat_prod', defaultValue: '35.171.23.49', description: 'Production Server')
  }
  triggers {
    pollSCM('* * * * *')
  }

  stages {
    stage('Build') {
      steps {
        bat 'mvn clean package'
      }
      post{
        success{
          echo 'Now Archiving...'
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }
    }
    stage('Deployments') {
      parallel{
        stage('Deploy to Staging') {
          steps {
            //bat 'winscp -i "C:\\Users\\Dioni\\Dropbox\\Aws\\key\\tomcat-jenkins-study-stag.pem" **/target/*.war ec2-user@${params.tomcat_dev}: /var/lib/tomcat8/webapps'
          bat "Winscp -i /Users/Dioni/Dropbox/Aws/key/tomcat-jenkins-study-stag.pem **/target/*.war ec2-user@${params.tomcat_dev}: /var/lib/tomcat8/webapps"
          }
        }
        stage('Deploy to Production') {
          steps {
            bat "Winscp -i /Users/Dioni/Dropbox/Aws/key/tomcat-jenkins-study.pem **/target/*.war ec2-user@${params.tomcat_prod}: /var/lib/tomcat8/webapps"
          }
        }
      }

    }
  }
}
