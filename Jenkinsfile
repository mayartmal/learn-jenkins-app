pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '7e69fc94-9407-43dc-8dda-480e7b9185c5'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        REACT_APP_VERSION = "0.0.0.$BUILD_ID"
    }

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
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright LOCAL HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
                    }
                    
                }
            }
        }

        
        
        stage('Deploy Staging') {
            agent {
                docker {
                    // image 'node:18-alpine'
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Deploy ... ... ..."
                    # npm install netlify-cli 
                    npm install netlify-cli@20.1.1 node-jq
                    node_modules/.bin/netlify --version
                    echo "Deploying to staging. Site id $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --json > deploy-output.json
                    node_modules/.bin/netlify deploy --dir=build --json

                    
                '''
                script {
                    env.STAGING_URL = sh(script: "node_modules/.bin/node-jq -r '.deploy_url' deploy-output.json", returnStdout: true)
                }
            }

        }

        stage('Staging E2E') {
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

            environment {
                CI_ENVIRONMENT_URL = "${env.STAGING_URL}"
            }

            
            steps{
                sh '''
                    npx playwright test --reporter=html
                '''
            }

            post {
                always {
                    sh '''
                        echo "Running e2e post actions... "
                    '''
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright STAGING HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                }
            }
            
        }


        

        // stage("Approval") {
        //     steps{
        //        timeout(time: 1, unit: 'HOURS') {
                    
        //            input message: 'Ready for deploy?', ok: 'Yes, I am sure I want to deploy'
        //         }
        //     }
        // }
        
        
        stage('Deploy Prod') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Deploy ... ... ..."
                    # npm install netlify-cli
                    npm install netlify-cli@20.1.1
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Site id $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }

        stage('Prod E2E') {
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

            environment {
                CI_ENVIRONMENT_URL = 'https://mayartmal.netlify.app'
            }

            
            steps{
                sh '''
                    npx playwright test --reporter=html
                '''
            }

            post {
                always {
                    sh '''
                        echo "Running e2e post actions... "
                    '''
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright PROD HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                }
            }
            
        }




        stage('Finalize'){
            steps {
                sh '''
                    echo everithing ok. it is finalization
                '''
            }
            post {
                success {
                sh '''
                    echo "ALL FINISHED"
                '''

                }
            }  
        }
    }
}