pipeline {
    agent { label 'ubuntu' }
    tools {
        maven 'default'
    }
    parameters {
        string(defaultValue: "DEV", description: 'What environment?', name: 'DEV_PROJECT')
    }
    options {
        buildDiscarder(logRotator(daysToKeepStr: '3'))
      }

    stages {
        stage('Checking Parameters') {
            steps {
                sh "echo DEV_PROJECT is ${params.DEV_PROJECT}"
            }
        }
        stage('Build') {
             steps {
                 withMaven(
                    maven: 'default')
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
