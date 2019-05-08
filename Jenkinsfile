pipeline {
    agent any
    tools {
        maven 'default'
    }
    parameters:
        string(defaultValue: "DEV", description: 'What environment?', name: 'DEV_PROJECT')

    stages:{
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
