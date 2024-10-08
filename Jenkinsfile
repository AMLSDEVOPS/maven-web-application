def gv

pipeline {
    agent any
    parameters {
        string (name: 'VERSION', defaultValue: '', description: 'Version to deploy')
        choice (name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }
    
    
    tools {
        maven 'maven3'
    }
    
    environment {
        NEW_VERSION = '1.3.0'
       // SERVER_CREDENTIALS = credentials('server-credentials-id') // Replace with actual credentials ID
    }

    stages {
        stage('Cloning Repo') {
            steps {
                git credentialsId: 'mvenkatgit', url: 'https://github.com/MithunTechnologiesDevOps/maven-web-application.git'
            }
        }

        stage('Running the Unit Tests') {
            steps {
                echo "Building branch ${env.BRANCH_NAME} for PR ${env.CHANGE_ID ?: 'N/A'}"
                // Added null check for CHANGE_ID to avoid issues if it's not set
            }
        }

        stage('Testing the Application') {
            steps {
                echo "Running tests for Build Number: ${env.BUILD_NUMBER}"
                echo "Building the Version of ${NEW_VERSION}"
                 echo "Building the Version of ${VERSION}"
            }
        }
        
        stage('Application') {
            steps {
                echo "Running tests for Build Number: ${env.BUILD_NUMBER}"
                echo "Building the Version of ${NEW_VERSION}"
                 echo "Building the Version of ${params.VERSION}"
            }
        }

        stage('Deploying the Application') {
            when {
                expression {
                    return env.BRANCH_NAME == 'dev' || env.BRANCH_NAME == 'master'
                }
            }
            steps {
                echo "Deploying to branch ${env.BRANCH_NAME}"
                // Uncomment and update this block if you need credentials and a deployment script
                /*
                withCredentials([usernamePassword(credentialsId: 'system-user', usernameVariable: 'USER', passwordVariable: 'PWD')]) {
                    sh "some script here ${USER} ${PWD}"
                }
                */
            }
        }
        
        stage('init') {
            steps {
                gv = load "script.groovy"
            }
        }
        stage('Build') {
            steps {
               script {
                   gv.buildApp()
               }
            }
        }
        
        stage('Test') {
            steps {
               script {
                   gv.testApp()
               }
            }
        }
        
        stage('Deploying') {
            steps {
               script {
                   gv.DeployingApp()
               }
            }
        }

        stage('Workspace Cleanup') {
            steps {
                cleanWs()
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
        }

        success {
            echo 'Pipeline succeeded'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}
