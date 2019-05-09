pipeline {
    agent { label 'ubuntu' }
    tools {
        maven 'default'
    }
    parameters {
        string(defaultValue: "DEV", description: 'What environment?', name: 'DEV_PROJECT')
    }
    environment {
            VAULT_APP_ID_PROD   = credentials('178659f2-9e6f-478b-8385-daabbdd8a88a')
    }
    options {
        buildDiscarder(logRotator(daysToKeepStr: '3'))
      }

    stages {
        stage('Checking Parameters') {
            steps {
                sh "echo DEV_PROJECT is ${params.DEV_PROJECT}"
                sh "echo VAULT_APP_ID_USER is $VAULT_APP_ID_PROD_USR"
                sh "echo VAULT_APP_ID_PWD is ${VAULT_APP_ID_PROD_PSW}"
                // cleanWs()
            }
        }
        stage('Build') {
             steps {
                 sh 'mvn -B -DskipTests clean package'
              }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
