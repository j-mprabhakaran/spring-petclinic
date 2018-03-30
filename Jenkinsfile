pipeline {
    agent { label 'linux' }
    stages {
        stage ('Checkout') {
          steps {
            git 'https://github.com/jmprabhakaran/spring-petclinic.git'
          }
        }
        stage('Build') {
            agent { docker 'maven:3.5-alpine' }
            steps {
                sh 'mvn clean package'
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
          steps {
            input 'Do you approve the deployment?'
            sh 'scp target/*.jar ec2-user@52.206.180.13:/opt/pet/'
            sh "ssh ec2-user@52.206.180.13 'nohup java -jar /opt/pet/spring-petclinic-1.5.1.jar &'"
          }
        }        
    }
}
