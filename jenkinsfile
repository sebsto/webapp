pipeline {
    agent any
parameters { 
         string(name: 'tomcat_dev', defaultValue: '13.59.98.137', description: 'Staging Server') 
         string(name: 'tomcat_prod', defaultValue: '18.218.209.248', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
     
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package' 
                }
                post {
                  success
                  {
                    echo 'Now Archiving...'
                          archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deployments'){
            parallel{
        stage('Deploy to Staging') {
         steps {
                        sh "pwd && ls -la"
                        sh "sudo scp -i /Repo1/devops.pem ./target/*.war ec2-user@${params.tomcat_dev}:/opt/apache-tomcat-7.0.53/webapps"
                        
                    }
               }
          stage ("Deploy to Production"){
                    steps {
                        sh "scp -i  /Repo1/devo.pem ./target/*.war ec2-user@${params.tomcat_prod}:/root/apache-tomcat-7.0.53/webapps"
                    }
              }
         }
    }
 }
}
