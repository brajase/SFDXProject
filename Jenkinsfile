#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID ='a1304981-ba49-49c1-95a3-8ffc6e027449'
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println 'User Name' 
    println HUB_ORG
    println 'instance URL' 
    println SFDC_HOST
    println 'ClientID' 
    println CONNECTED_APP_CONSUMER_KEY


    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }


        stage('Authorize DevHub') {            

                rc = command "sfdx force:auth:jwt:grant  --clientid 3MVG9szVa2RxsqBYXscs6zhOGSsPG_Pmr3Ik2ceNuLNQLAIsGwRfJ96YGtZRmbC7W62DhZPzEc3t.4RpkElFq --jwtkeyfile C:\\MyApplications\\Jenkins\\keys\\kaaladev2.PEM --username chandar_bala@hotmail.com.shield --instanceurl https://login.salesforce.com --setdefaultusername"
        }
            if (rc != 0) { error 'authorization failed' }

			println rc	
    
}

