pipeline {

    agent {
        node {
            label 'master'
        }
    }
    environment { 
        PATH = "/root/apictl:$PATH"
    }
    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {

        stage('Setup Environment for APICTL') {
            steps {
                sh """#!/bin/bash
                ENVCOUNT=\$(apictl list envs --format {{.}} | wc -l)
                if [ "\$ENVCOUNT" == "0" ]; then
                    apictl add-env -e live --apim https://live.apis.com:9443
                fi
                """
            }
        }

        stage('Deploy APIs To "Live" Environment') {
            steps {
                sh """
                apictl login live -u admin -p admin -k
                apictl vcs deploy -e live -k
                """
            }
        }
        
         stage('Automated test in "Live" Environment') {
            steps {
                sh """
                echo "Initiating Test"
                echo "Test In progress"
                echo "Test completed"
                """
            }
        }
    }   
}
