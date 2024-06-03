pipeline {
    agent any
    tools {
	    maven "MAVEN3"
	    jdk "OracleJDK8"
	}

    stages{
        stage('Fetch code') {
          steps{
              git branch: 'main', url:'https://github.com/kishanecosmob/cicd-test-latest.git'
          }  
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Test'){
            steps {
                sh 'mvn test'
            }

        }

        stage('Checkstyle Analysis'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Sonar Analysis') {
            environment {
                scannerHome = tool 'sonar4.7'
            }
            steps {
               withSonarQubeEnv('sonar') {
                   sh '''${scannerHome}/bin/sonar-scanner \
  		   -Dsonar.projectKey=kishan_new-june \
                   -Dsonar.sources=. \
                   -Dsonar.host.url=http://172.16.18.140 \
                   -Dsonar.login=10f09327a47a05c87246617879e872917362f42d

               }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage("UploadArtifact"){
            steps{
                nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: '172.16.18.116:8081',
                  groupId: 'QA',
                  version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                  repository: 'kishan_new-june',
                  credentialsId: 'nexuslogin',
                  artifacts: [
                    [artifactId: 'kishan_new-june',
                     classifier: '',
                     file: 'target/kishan_new-june_2.war',
                     type: 'war']
                  ]
                )
            }
        }
    }
    
}
