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

        stage('Deployments') {
            steps {
                sh "sudo scp -i /var/lib/jenkins/secrets/envstage/heroiot-envstage-devops.pem /var/lib/jenkins/workspace/java-springboot-app/target/*.war centos@${params.tomcat_devops}:/opt/tomcat/webapps"
            }
        }
        stage('restart tomcat service @webserver-1a') {
            steps {
                sshagent(credentials: ['sshagent_envstage_devops']) {
                    sh 'ssh -o StrictHostKeyChecking=no -l centos 10.5.30.150 <<EOF
                    pwd
                    hostnamectl
                    sudo systemctl stop tomcat
                    sleep 10
                    sudo systemctl start tomcat
                    EOF'
                }
            }
        }
    }
}
