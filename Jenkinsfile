#!groovy

def kubernetes
def stash

pipeline {

    agent {
        label 'java11'
    }

    tools {
        jdk 'java11'
    }

    stages {
        
        stage('Clone') {
            checkout scm
        }
        
        stage("Clear Expectations") {
            steps {
                sh "curl --insecure -v -X PUT \"https://service.mockserver.fdv.servicedesk.k8s.dev.gt.intranet.pags/mockserver/reset\""
                
                sh "curl --insecure -v -X PUT \"https://service.mockserver.fdv.servicedesk.k8s.dev.tb.intranet.pags/mockserver/reset\""
            }
        }

        stage("Init Expectations") {
            steps {
                sh "curl --insecure -v -X PUT \"https://service.mockserver.fdv.servicedesk.k8s.dev.gt.intranet.pags/mockserver/openapi\" -d '{ \"specUrlOrPayload\": \"https://raw.githubusercontent.com/mock-server/mockserver/master/mockserver-integration-testing/src/main/resources/org/mockserver/mock/openapi_petstore_example.json\" }'"
                
                sh "curl --insecure -v -X PUT \"https://service.mockserver.fdv.servicedesk.k8s.dev.tb.intranet.pags/mockserver/openapi\" -d '{ \"specUrlOrPayload\": \"https://raw.githubusercontent.com/mock-server/mockserver/master/mockserver-integration-testing/src/main/resources/org/mockserver/mock/openapi_petstore_example.json\" }'"
            }
        }
    }

    post {
        always {
            deleteDir()
        }
    }
}
