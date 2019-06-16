pipeline {
    agent any

    parameters {
        string(name: 'tomcat_dev', defaultvalue: '18.223.120.128', description: 'staging server')
        string(name: 'tomcat_prod', defaultvalue: '18.224.23.3', description: 'production server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage ('Build') {
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

        stage ('Deployments') {
            parallel {
                stage ('Deploy to Staging') {
                    steps {
                        sh "scp -i ~/Users/claytonlin/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
                stage ('Deploy to Prod') {
                    steps {
                        sh "scp -i ~/Users/claytonlin/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}