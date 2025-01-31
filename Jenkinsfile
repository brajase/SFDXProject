pipeline {
    agent any

    options { 
        buildDiscarder(logRotator(numToKeepStr: '5')) 
        timeout(time: 1, unit: 'HOURS') 
    }

    tools {     
        'com.cloudbees.jenkins.plugins.customtools.CustomTool' 'sfdx'
    }

    environment {        
                def USER_NAME='chandar_bala@hotmail.com.shield'
                def INSTANCE_URL='https://login.salesforce.com' 
                def TEST_LEVEL='RunLocalTests'
            }

    triggers { 
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {

            steps {
                echo "Running Build # '${env.BUILD_ID}' on '${env.JENKINS_URL}'"
                // when running in multi-branch job, one must issue this command
                echo 'Checking out the source code from GIT'
                checkout scm
              // checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/brajase/SFDXProject.git']]])
            }
        }
        
        stage('Authorize'){
           environment {
                CLIENT_ID = credentials('dev.client.id')
                CLIENT_SECRET = credentials('a1304981-ba49-49c1-95a3-8ffc6e027449')
           }
            steps{

                echo 'Authorize Salesforce Org...'
                script{                   
                       
                        //rc = bat(returnStatus:true , script: "sfdx force:auth:jwt:grant --clientid $CLIENT_ID --jwtkeyfile C:\\MyApplications\\Jenkins\\keys\\kaaladev2.PEM --username $USER_NAME --instanceurl $INSTANCE_URL --setdefaultusername")
                       // rc = bat(returnStatus:true , script: "sfdx force:auth:jwt:grant --clientid $CLIENT_ID --jwtkeyfile $CLIENT_SECRET --username $USER_NAME --instanceurl $INSTANCE_URL --setdefaultusername")
                        rc= command "sfdx force:auth:jwt:grant --clientid $CLIENT_ID --jwtkeyfile $CLIENT_SECRET --username $USER_NAME --instanceurl $INSTANCE_URL --setdefaultusername"
                        echo 'Exited script run'                         
                }
            }
        }
        
        stage('Test') {
            steps {
                echo 'Testing..'
                echo 'Running PMD...'
            }
        }
        
        stage('Deploy') {
            when {
                branch 'master'
                //environment name: 'DEPLOY_TO', value: 'production'                
            }
            steps {
                echo 'Deploying....'                    
                script{
                    rc= bat(returnStatus:true, script: "sfdx force:source:deploy -x manifest/package.xml")
                    if (rc != 0) {
                        error 'Salesforce deployment failed.'
                    }
                }

            }            
        }

            // -------------------------------------------------------------------------
            // Run unit tests
            // -------------------------------------------------------------------------

        stage('Run Tests In Sandbox Org') {
            steps {
                echo 'Run unit tests in Saleforce Org'
                script {
                    rc = command "sfdx force:apex:test:run  --wait 10 --resultformat tap --codecoverage --testlevel $TEST_LEVEL"
                    if (rc != 0) {
                        error 'Salesforce unit test run in test scratch org failed.'
                    }
                }
            }
        }
        
    }

    post {
        always {
            echo 'Deleting workspace ..' 
            //deleteDir()
        }
        success {
            echo "Build # '${env.BUILD_ID}' was successful"
            //mail to: 'chandar_bala@hotmail.com', subject: 'The Pipeline was successul', body: 'Shared lib build and deploy was success for master branch.'
        }
        failure {
            echo "Build # '${env.BUILD_ID}' failed!"
        }
    }
}

def command(script) {
    if (isUnix()) {
        return sh(returnStatus: true, script: script);
    } else {
        return bat(returnStatus: true, script: script);
    }
}