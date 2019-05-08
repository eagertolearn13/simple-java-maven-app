pipeline {
    agent any
    tools {
        maven 'default'
    }
    stages {
        stage('Prepping for the build') {
            steps{
                parameters:
                    [$class: 'TextParameterDefinition', defaultValue: 'dev', description: 'dev env', name: 'DEV_PROJECT']
            }

        }
        stage('Build') {
            steps {
                sh "DEV_PROJECT is ${params.DEV_PROJECT}"
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
