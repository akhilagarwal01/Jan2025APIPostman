pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo "Building the war"
            }
        }

        stage('Deploy to QA') {
            steps {
                echo "Deploying to QA"
            }
        }

        stage('Pull Docker Images') {
            parallel {
                stage('Pull GoRest Image') {
                    steps {
                        bat 'docker pull akhilagarwal012/bookingapitest:1.0'
                    }
                }
                
                stage('Pull Booking Image') {
                    steps {
                        bat 'docker pull akhilagarwal012/datadriventest:1.0'
                    }
                }
                
                stage('Pull ContactApi docker image'){
                    steps{
                        bat 'docker pull akhilagarwal012/contactapitest:1.0'
                    }
                }
            }
        }

       stage('Prepare Newman Results Directory') {
            steps {
                bat '''
                if not exist "newmanSelf" (
                    mkdir "newmanSelf"
                )
                '''
            }
        }

        stage('Run API Test Cases in Parallel') {
            parallel {
                stage('Run GoRest Tests') {
                    steps {
                        bat 'docker run --rm -v "%cd%"\\newmanSelf:/app/results akhilagarwal012/datadriventest:1.0'
                    }
                }
                
                stage('Run Booking Tests') {
                    steps {
                        bat 'docker run --rm -v "%cd%"\\newmanSelf:/app/results akhilagarwal012/bookingapitest:1.0'
                    }
                }
                
                stage('Run contact Tests') {
                    steps {
                        bat 'docker run --rm -v "%cd%"\\newmanSelf:/app/results akhilagarwal012/contactapitest:1.0'
                    }
                }
            }
        }

        stage('Publish HTML Extra Reports') {
            parallel {
                stage('Publish GoRest Report') {
                    steps {
                        publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newmanSelf',
                            reportFiles: 'htmlextra.html',
                            reportName: 'DD API Report',
                            reportTitles: ''
                        ])
                    }
                }
                
                stage('Publish Booking Report') {
                    steps {
                        publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newmanSelf',
                            reportFiles: 'BookingapiResult.html',
                            reportName: 'Booking API Report',
                            reportTitles: ''
                        ])
                    }
                }
                
                stage('Publish Contact API Report') {
                    steps {
                        publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newmanSelf',
                            reportFiles: 'ContactsApiResult.html',
                            reportName: 'contact API Report',
                            reportTitles: ''
                        ])
                    }
                }
            }
        }

        stage('Deploy to PROD') {
            steps {
                echo "Deploying to PROD"
            }
        }
    }
}
