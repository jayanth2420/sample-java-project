pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from Git repository
                git 'https://github.com/salvadisravan/sample-java-project.git'
            }
        }
        stage('Build') {
            steps {
                // Build your project (e.g., compile code, package artifacts)
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                echo "Run tests (e.g., unit tests, integration tests)"
                sh 'mvn test'
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
