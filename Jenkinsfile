#!/usr/bin/env groovy

pipeline {
    agent any
/*
    environment {
        NAME = readMavenPom().getArtifactId()
    }
*/
    options {
        timeout(time: 1, unit: 'HOURS')
        buildDiscarder(logRotator(daysToKeepStr: '10', numToKeepStr: '5', artifactNumToKeepStr: '2'))
    }

    stages {
        stage("ssh-test") {
            steps {
                sshagent(['jenkins']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ttwcsop@10.1.1.3 'ls -lrt'
                    """
                }
            }
        }
        
/*
        stage("Git Checkout") {
            steps {
                git branch: 'develop',
                credentialsId: 'jenkins', url: 'https://gitlab.gov/ih/ih-por.git'
            }
        }
        stage('Maven build') {
            steps {
                sh "mvn clean package"
                sh "mv target/*.war target/UI.war"
            }
        }
        stage("deploy-dev") {
            steps {
                sshagent(['user-id-tomcat-dev']) {
                    sh """
                    scp -o StrictHostKeyChecking=no  target/UI.war  root@192.168.1.000:/opt/tomcat/webapps/
                    ssh root@192.168.1.000 /opt/tomcat/bin/shutdown.sh
                    ssh root@192.168.1.000 /opt/tomcat/bin/startup.sh
                    """
                }
            }
        }
*/

    }
}
