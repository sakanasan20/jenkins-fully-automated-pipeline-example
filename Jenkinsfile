pipeline {
    agent any
    
    parameters {
        string(name: 'tomcat_dev', defaultValue: '35.78.198.248', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '35.78.104.46', description: 'Production Server')
    }
    
    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                build job: 'maven-project-package'
            }
        }
        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        sh "scp -i /home/nick/jenkins-workspace/tomcat-ec2.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }
                stage('Deploy to Production') {
                    steps {
                        sh "scp -i /home/nick/jenkins-workspace/tomcat-ec2.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                } 
            }
            
        }
    }
}
