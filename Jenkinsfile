pipeline {
    agent any

    stages {
        // This is a comment
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                // cleanWs()
                sh '''
                     echo "Building ... ... ..."
                     ls -la
                     node --version
                     npm --version
                     npm ci
                     npm run build
                     echo "Building almost complete ... ... ..."
                     ls -la
                '''
            }
        }
        stage('Run Tests'){
            parallel {                
                stage('Unit Test') {
                    /*
                        this 
                        is
                        a comment
                    */
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    
                    steps{
                        sh '''
                            echo "Test stage in progress ... ... ..."
                            test -f build/index.html
                            npm test
                            # this is a shell  commands comment
                        '''
                    }
                    post {
                        always {
                            sh '''
                                echo "Running unit tests post actions... "
                            '''
                            junit 'jest-results/junit.xml'     
                        }
                    }                    
                }
                stage('E2E') {
                    /*
                        this 
                        is
                        a comment
                    */
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                            reuseNode true
                            // args '-u root:root' not a good pracrice 
                        }
                    }
                    
                    steps{
                        sh '''
                            npm install serve
                            node_modules/.bin/serve -s build & 
                            sleep 10
                            npx playwright test --reporter=html
                        '''
                    }

                    post {
                        always {
                            sh '''
                                echo "Running e2e post actions... "
                            '''
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
                    }
                    
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
                     echo "Deploy ... ... ..."
                    npm install -g netlify-cli
                    netlify --version
                '''
            }
        }

        stage('Finalize'){
            steps {
                sh '''
                    echo everithing ok. it is finalization
                '''
            }
            post {
                always {
                sh '''
                    echo "ALL FINISHED"
                '''

                }
            }  
        }
    }
}