pipeline {
    agent any

   
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                // when running in multi-branch job, one must issue this command
                echo 'Checking out the source code from GIT'
                checkout scm
            }
        }
        stage('Authorize Salesforce Org'){
            steps{
                echo 'Authorize Salesforce Org...'
                echo 'SFDX_HOME is '
                echo "${env.SFDX_HOME}"
                script{                   
                        rc = bat(returnStatus:true , script: "C:\\delMe\\sfdx\\bin\\sfdx force:auth:jwt:grant --clientid 3MVG9szVa2RxsqBYXscs6zhOGSsPG_Pmr3Ik2ceNuLNQLAIsGwRfJ96YGtZRmbC7W62DhZPzEc3t.4RpkElFq --jwtkeyfile C:\\MyApplications\\Jenkins\\keys\\kaaladev2.PEM --username chandar_bala@hotmail.com.shield --instanceurl https://login.salesforce.com --setdefaultusername")
                        echo 'Exited script run'
                        print rc                           
                                   
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
                    rc = command "C:\\delMe\\sfdx\\bin\\sfdx force:source:deploy -x manifest\\package.xml --targetusername chandar_bala@hotmail.com.shield"
                    if (rc != 0) {
                        error 'Salesforce push failed.'
                    }
                }
            }            
        }
    }
}