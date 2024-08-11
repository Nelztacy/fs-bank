pipeline {
    agent any
    
    tools{
        jdk 'jdk17'
        nodejs 'node16'
        
    }
    
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Nelztacy/fs-bank.git'
            }
        }
        
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./app/backend --disableYarnAudit --disableNodeAudit', odcInstallation: 'DC'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs ."
            }
        }
        
        // stage('SONARQUBE ANALYSIS') {
        //     steps {
        //         withSonarQubeEnv('sonar') {
        //             sh " $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=fs-bank -Dsonar.projectKey=fs-bank "
        //         }
        //     }
        // }
        
        
         stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        
        stage('Backend') {
            steps {
		dir('/var/lib/jenkins/workspace/fs-bank/app/backend') {
                    sh "npm install"
                }
            }
        }
        
        stage('frontend') {
            steps {
		dir('/var/lib/jenkins/workspace/fs-bank/app/frontend') {
                    sh "npm install"
                }
            }
        }
        
        stage('Deploy to Container') {
            steps {
                sh "npm run compose:up -d"
            }
        }
    }
}