pipeline {
    agent any
    tools {
        maven 'default'
    }
    parameters {
        string(defaultValue: "DEV", description: 'What environment?', name: 'DEV_PROJECT')
    }

    stages {
        stage('Checking Parameters') {
            steps {
                sh "echo DEV_PROJECT is ${params.DEV_PROJECT}"
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
