pipeline {
    agent any

    options { 
        buildDiscarder(logRotator(numToKeepStr: '5')) 
    }

    tools {     
        'com.cloudbees.jenkins.plugins.customtools.CustomTool' 'sfdx'
    }

    environment {
                 def USER_NAME='chandar_bala@hotmail.com.shield'
                 def INSTANCE_URL='https://login.salesforce.com'              
            }

    stages {
       
    
        stage('Build') {

            steps {
                echo "Running Build # '${env.BUILD_ID}' on '${env.JENKINS_URL}'"
                // when running in multi-branch job, one must issue this command
                echo 'Checking out the source code from GIT'
                checkout scm
            }
        }
        
        stage('Authorize'){
           environment {
                CLIENT_ID = credentials('dev.client.id')
           }
            steps{
                echo 'Authorize Salesforce Org...'
                script{                   
                        //rc = bat(returnStatus:true , script: "sfdx force:auth:jwt:grant --clientid 3MVG9szVa2RxsqBYXscs6zhOGSsPG_Pmr3Ik2ceNuLNQLAIsGwRfJ96YGtZRmbC7W62DhZPzEc3t.4RpkElFq --jwtkeyfile C:\\MyApplications\\Jenkins\\keys\\kaaladev2.PEM --username chandar_bala@hotmail.com.shield --instanceurl https://login.salesforce.com --setdefaultusername")
                        rc = bat(returnStatus:true , script: 'sfdx force:auth:jwt:grant --clientid $CLIENT_ID --jwtkeyfile C:\\MyApplications\\Jenkins\\keys\\kaaladev2.PEM --username $USER_NAME --instanceurl $INSTANCE_URL --setdefaultusername')
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
            steps {
                echo 'Deploying....'

                script{
                    if (rc != 0) {
                        error 'Salesforce push failed.'
                    }
                }
            }            
        }
        
    }

    post {
        always {
            echo 'Inside always block ..'
        }
        success {
            echo "Build # '${env.BUILD_ID}' was successful"
            //mail to: 'chandar_bala@hotmail.com', subject: 'The Pipeline was successul', body: 'Shared lib build and deploy was success for master branch.'
        }
    }
}