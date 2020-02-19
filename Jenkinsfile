pipeline {
    agent any
    parameters {
         string(name: 'tomcat_devops', defaultValue: '10.5.30.150', description: 'devops tomcat server')
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }

                stage ('Deployments'){
                    steps {
                        sh "sudo scp -i /var/lib/jenkins/secrets/envstage/heroiot-envstage-devops.pem /var/lib/jenkins/workspace/java-springboot-app/target/*.war centos@${params.tomcat_devops}:/opt/tomcat/webapps"
                    }
                }
         }
}
