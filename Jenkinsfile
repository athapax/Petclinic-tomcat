pipeline {
    agent {
        label "linux-aws-agent"
    }
    stages {
        stage('Git Checkout') {
            steps {
             echo "Checking out our code"
            git branch: 'main', url: 'https://github.com/athapax/Petclinic-tomcat.git'
            }
        }
        stage('Validate') {
            steps {
                echo "Validating on our maven build"
                sh "mvn clean validate"
            } 
        }
        stage('Test') {
            steps {
                echo "Running test on our maven build"
                sh "mvn clean test"
            } 
        }
        stage('Build') {
            steps {
                sh "mvn clean install"
            } 
        }
        stage('Deploy') {
            steps {
                 echo "Deploying our war file into our tomcat prod server"
                sshagent(['tomcat-pipeline']) {
                    sh "scp -o StrictHostKeyChecking=no target/petclinic.war tomcat@54.161.48.79:/opt/tomcat/webapps"
                   }
                }
            } 
        }
    }

