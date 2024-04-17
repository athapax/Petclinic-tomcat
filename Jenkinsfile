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
        stage ('Complie') {
             steps {
              echo "Compliling our code"
             sh "mvn clean compile"
             }
         }
         stage('Validate') {
            steps {
                echo "Validating on our maven build again"
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
        stage('Approval') {
            steps {
                script {
                    // Request approval from a specific user
                    def userInput = input(
                        id: 'userInput', 
                        message: 'Do you want to proceed with deployment?', 
                        parameters: [
                            [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Proceed with deployment?', name: 'Proceed']
                        ]
                    )
                    // Check if user approved the deployment
                    if (!userInput.Proceed) {
                        error('Deployment aborted by user.')
                    }
                }
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

