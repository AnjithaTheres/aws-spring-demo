pipeline {
  tools {
    maven 'Maven3.8.3'
  }
    agent any
    stages {
        stage('Clean') {
            steps {
                echo 'Cleaning..'
                bat 'mvn -B -DskipTests clean'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                bat 'mvn test'
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
                    bat 'echo "Packaging done"'
                }
                failure{
                    bat 'echo "Packaging failure"'
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
                    bat 'echo "Uploaded to AWS"'
                }
                failure{
                    bat 'echo "failure"'
                }
            }
        
        }
    }
}