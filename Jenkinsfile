#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH
    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Authorize DevHub') {
            if (isUnix()) {
                rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST} --setalias hextensaicicdpoc"
            }else{
                 rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST} --setalias hextensaicicdpoc"
            }
            if (rc != 0) { error 'hub org authorization failed' }

            // need to pull out assigned username

            println('Completed : Authorize DevHub!')           
        }
        
        stage('Create Test Scratch Org') {
            if (isUnix()) {
                rmsg1 = sh returnStdout: true, script: "${toolbelt} force:org:display -u hextensaicicdpoc"
                rmsg2 = sh returnStdout: true, script: "${toolbelt} config:set defaultusername=hextensaicicdpoc --global"
                rmsg3 = sh returnStdout: true, script: "${toolbelt} force:config:set defaultdevhubusername=hextensaicicdpoc"
                rmsg4 = sh returnStdout: true, script: "${toolbelt} config:list"
                rmsg5 = sh returnStdout: true, script: "${toolbelt} force:org:open"
                rmsg6 = sh returnStdout: true, script: "${toolbelt} force:org:list"
            }else{
                rmsg1 = bat returnStdout: true, script: "\"${toolbelt}\" force:org:display -u hextensaicicdpoc"
                rmsg2 = bat returnStdout: true, script: "\"${toolbelt}\" config:set defaultusername=hextensaicicdpoc --global"
                rmsg3 = bat returnStdout: true, script: "\"${toolbelt}\" force:config:set defaultdevhubusername=hextensaicicdpoc"
                rmsg4 = bat returnStdout: true, script: "\"${toolbelt}\" config:list"
                rmsg5 = bat returnStdout: true, script: "\"${toolbelt}\" force:org:open"
                rmsg6 = bat returnStdout: true, script: "\"${toolbelt}\" force:org:list"
            }
            if (rmsg != 0) { error 'Create Test Scrathc Org failed' }

            println('Completed : Test Scratch Org!')           
        }
        
          stage('Push To Test Org') {

            println('Completed : Push to Test Org!')
          }
        
          stage('Run Tests In Test Scratch Org') {

            println('Completed : Push to Test Org!')
          }
             
    }
}
