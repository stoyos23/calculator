pipeline {

    agent any

    stages{
        
        stage("Install") {
            steps{
                sh 'npm install'
            }
        }

        stage("Build") {   
            steps{

                sh 'npm run build'
            }
        }

            stage('Sonarqube') {
                environment {
                    def scannerHome = tool 'SonarQubeScanner'
                    }
                steps {
                    withSonarQubeEnv('SonarQubeServer') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                    timeout(time: 3, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }

		 stage("deploy") {      
            steps{
                   sh 'scp -r -v -o StrictHostKeyChecking=no build/* vagrant@192.168.10.10:/var/www/html'
            }
        }

    }
}