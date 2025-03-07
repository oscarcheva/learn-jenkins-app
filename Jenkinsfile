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
        
        stage('With Docker') {
            agent {
                docker {
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

        stage('Build') {
            agent {
                docker {
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
/*         stage('Run Tests') {
            parallel { 
                stage('Test') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                        echo "Test Stage"
                        test -f build/index.html
                        node --version
                        npm --version
                        npm test
                        '''
                    }
                }
                stage('E2E') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.51.0-noble'
                            reuseNode true
                            //args '-u root:root'
                        }
                    }
                    steps {
                        sh '''
                        npm install serve
                        node_modules/.bin/serve -s build &
                        sleep 10
                        npx playwright test --reporter=html
                        '''
                    }
                }
            }
        } */

        stage('Test') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                        echo "Test Stage"
                        test -f build/index.html
                        node --version
                        npm --version
                        npm test
                        '''
                    }
                    post {
                        always {
                            junit 'jest-results/junit.xml'
                        }
    }
                }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                npm install netlify-cli
                netlify --version
                '''
            }
        }
    }

}