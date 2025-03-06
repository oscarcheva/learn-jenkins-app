pipeline {   
    agent any

    stages {
        
        stage('Without Docker') {
            steps {
                sh '''
                echo "Hello World without Docker"
                ls -la
                touch container-no.txt
                '''
            }
        }
        
        stage('With Docker'){
        
            agent{
                
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                
                sh '''
                echo "Hello World with Docker"
                npm --version
                ls -la
                touch container-yes.txt
                echo "Hello Docker file" >> container-yes.txt
                cat container-yes.txt
                '''
            }
        }

        stage('Build'){
        
            agent{
                
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}

