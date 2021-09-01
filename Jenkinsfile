pipeline {
    agent any

    options {
        timestamps() // Add timestamps to logging
        timeout(time: 12, unit: 'HOURS') // Abort pipleine

        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        disableConcurrentBuilds()
    }
    environment {
        PATH = "/usr/local/bin:$PATH"
    }
    parameters {
        choice(name: 'TestName',
               choices: [
                   'Basic GoPath',
                   'Allocation Mode',
                   'Allocation Required',
                   'Auto GPS-SDL',
                   'Block IV Non-GPS',
                   'Specific Tasks',
                   'ALL'
               ],
               description: 'Select a Testcase to run')
    }

    stages {
        stage('Init') {
            steps {
                echo "Stage: Init"
                echo "branch=${env.BRANCH_NAME}, test=${params.TestName}"
            }
        }
        stage('Common Config') {
            steps {
                echo 'Stage: Common Config'
                // Checkout repo with common config files/scripts to 'common' folder
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'common']],
                          submoduleCfg: [], userRemoteConfigs: [[url: 'file:///home/jenkins/gitrepos/cicd-common']]
                         ])
            }
        }
        stage('XXX') {
            steps {
                echo "Stage: Test Manager Test"
                dir('common') {
                    // Transfer DX data from TTWCS to AM
                    sh '''
                    ./transfer_data_to_am.sh
                    ls -l
                    idtag=$(cat currentDxFile)
                    echo "Starting Analysis with Analysis idtag=${idtag}"
                    '''   
                }
                build(job: '/AnalysisMgr/main', parameters: [string(name: 'idtag', value: "${idtag}")], wait: true)   
            }        
        }
        stage('Cleanup') {
            steps {
                echo "Stage: Cleanup"
            }
        }
    }
    post {
        always {
            echo "post/always"
            //deleteDir() ////////////////////////////////////
        }
        success {
            echo "post/success"
        }
        failure {
            echo "post/failure"
        }
    }
}
