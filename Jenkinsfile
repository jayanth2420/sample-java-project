pipeline {
    agent any
    
    parameters {
        string(name: 'MAVEN_COMMAND', defaultValue: 'clean package', description: 'Maven command to run')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from Git repository
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'f0f7fd4c-11e6-4f16-8563-4f503e476d39', url: 'https://github.com/jayanth2420/sample-java-project.git']])
            }
        }
        stage('Build') {
            steps {
                // Build your project using the specified Maven command
                bat "mvn ${params.MAVEN_COMMAND}"
            }
        }
        stage('Test') {
            steps {
                echo "Run tests (e.g., unit tests, integration tests)"
               bat "mvn ${params.MAVEN_COMMAND}"
            }
        }
        stage('Deploy to QA') {
            steps {
                echo "Deploy to QA environment"
                //sh 'kubectl apply -f qa-deployment.yaml'
            }
            post {
                success {
                    // Notify success
                    echo 'Deployment to QA succeeded!'
                }
                failure {
                    // Notify failure
                    echo 'Deployment to QA failed!'
                }
            }
        }
        stage('Deploy to Production') {
            when {
                // Only deploy to production if previous stages were successful
                expression {
                    currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                echo "Deploy to production environment"
                //sh 'kubectl apply -f prod-deployment.yaml'
            }
            post {
                success {
                    // Notify success
                    echo 'Deployment to production succeeded!'
                }
                failure {
                    // Notify failure
                    echo 'Deployment to production failed!'
                }
            }
        }
    }
}
