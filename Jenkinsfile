pipeline {
    agent any
    
    stages {
        

        stage('Build') {
            steps {
                echo "Building the war"
            }
        }

        stage("Deploy to QA") {
            steps {
                echo "Deploying to QA"
            }
        }

        

        stage('Pull Docker Images') {
            steps {
                echo "pulling docker image "
                bat 'docker pull akhilagarwal012/datadriventest:1.0'
            }
                
            
        }


        stage('Run API Test Cases') {
            steps {
                bat 'docker run --rm -v "%cd%"/newman:/app/results akhilagarwal012/datadriventest:1.0'
            }
                
            
        }

        stage('Publish HTML Extra Reports') {
            
                    steps {
                        publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newman',
                            reportFiles: 'htmlextra.html',
                            reportName: 'Akhil_DD_Report',
                            reportTitles: ''
                        ])
                    }
                
                
            
        }

        stage("Deploy to PROD") {
            steps {
                echo "Deploying to PROD"
            }
        }
    }
}