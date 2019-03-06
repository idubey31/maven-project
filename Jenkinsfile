pipeline {
    agent any

    parameters {
         string(name: 'Tomcat-stage', defaultValue: '18.206.118.119', description: 'Staging Server')
         string(name: 'Tomcat-prod', defaultValue: '54.159.202.52', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "pwd"
                        sh "sudo -t scp -i ~/DevOpsKey.pem /var/lib/jenkins/workspace/AutomatedPipeline/webapp/target/*.war ec2-user@18.206.118.119:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "sudo -t scp -i ~/DevOpsKey.pem /var/lib/jenkins/workspace/AutomatedPipeline/webapp/target/*.war ec2-user@54.159.202.52:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
