pipeline{
    agent any
    environment{
	  PATH = "${PATH}:${tool name: 'Maven', type: 'maven'}/bin"
	}
    stages{
        stage('SCM Checkout'){
            steps{
                git url: 'https://github.com/yuvin-ram/jenkins',
                branch: 'master',
                credentialsId: 'github'
            }
        }

        stage('Maven Build'){
            steps{
                sh 'mvn clean package'
            }
        }

        stage('Deploy Dev'){
            steps{
                sshagent(['tomcat']) {
                    // stop tomcat
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@13.233.153.224 /opt/tomcat/bin/shutdown.sh"
                    // copy war file to remote tomcat
                    sh "scp target/6pmwebapp.war  ec2-user@13.233.153.224:/opt/tomcat/webapps/"
                    // start tomcat
                    sh "ssh ec2-user@13.233.153.224 /opt/tomcat/bin/startup.sh"
                } 
            }
        }
    }
}
