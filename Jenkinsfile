pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.212.33.158', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '100.24.52.110', description: 'Production Server')
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
                        sh "scp -i D:\Users\idubey\Desktop\DevOps\Jenkins\DevOpsKey.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i D:\Users\idubey\Desktop\DevOps\Jenkins\DevOpsKey.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}