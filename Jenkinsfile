pipeline {
  tools {
    maven 'Maven3.8.3'
  }
    agent any
    stages {
        stage('Clean') {
            steps {
                echo 'Cleaning..'
                sh 'mvn -B -DskipTests clean'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'mvn test'
            }
             post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Package') {
            steps {
                echo 'mvn install'
            }
            post {
                success{
                    sh 'echo "Packaging done"'
                }
                failure{
                    sh 'echo "Packaging failure"'
                }
            }
        }

        stage("Deploy to AWS"){
            steps{
                 withAWS(credentials:'puneetawscred', region:'us-east-1') {
                     s3Upload(workingDir:'target', includePathPattern:'**/*.war', bucket:'my-jenkinsangular1', path:'')
            }
            }
            post {
                success{
                    sh 'echo "Uploaded to AWS"'
                }
                failure{
                    sh 'echo "failure"'
                }
            }
        
        }
    }
}