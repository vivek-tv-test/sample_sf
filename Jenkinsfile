pipeline {
    agent any 

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Authenticate with Salesforce') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'salesforce-creds', passwordVariable: 'SFDC_PASSWORD', usernameVariable: 'SFDC_USERNAME')]) {
                        sh '''
                        sf login --instanceurl https://login.salesforce.com --clientid YOUR_CONSUMER_KEY --clientsecret YOUR_CONSUMER_SECRET --username $SFDC_USERNAME --password $SFDC_PASSWORD
                        '''
                    }
                }
            }
        }

        stage('Validate Deployment') {
            steps {
                sh 'sf deploy metadata preview -d path_to_your_metadata_folder'
            }
        }

        stage('Manual Deployment') {
            steps {
                input(message: 'Approve deployment?', ok: 'Deploy')
                sh 'sf deploy metadata -d path_to_your_metadata_folder'
            }
        }
    }

    post {
        always {
            echo 'Pipeline Execution Finished!'
        }
    }
