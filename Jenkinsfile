pipeline {
    agent any
    
    stages {
        
        stage('Code Quality Check') {
            steps {
                //running flake8 for code quality checks
                sh 'pip install flake8'
                sh 'flake8 .'
            }
        }
        
        stage('Unit Testing') {
            steps {
                //running unit tests using pytest
                sh 'pip install -r requirements.txt'
                sh 'pytest'
            }
        }
        
        stage('Build and Dockerize') {
            steps {
                //building docker image
                script {
                    docker.build('machine_learning_operations')
                }
                
                //pushing Docker image to Docker Hub
                script {
                    docker.withRegistry('https://registry.hub.docker.com', '1122') {
                        docker.image('machine_learning_operations').push('latest')
                    }
                }
            }
        }
    }
    
    post {
        success {
            //sending email notification upon successful execution
            emailext (
                subject: "CI/CD Pipeline Successful",
                body: "CI/CD pipeline has been successfully executed.",
                to: "kkamililyas@gmail.com"
            )
        }
    }
}
