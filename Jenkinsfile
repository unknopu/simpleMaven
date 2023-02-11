pipeline {
      agent any
      stages {
            stage('Init') {
                  steps {
                        echo 'Hi, this is Anshul from LevelUp360'
                        echo 'We are Starting the Testing'
                  }
            }
            stage('Build') {
                  steps {
                        echo 'Building Sample Maven Project'
                        sh 'mvn -f maven-samples/single-module/pom.xml clean package'
                  }
                  post {
                        success {
                              echo "Now Archiving the Artifacts...."
                              archiveArtifacts artifacts: '**/*.war'
                        }
                  }
            }
            stage('CODE ANALYSIS with SONARQUBE') {
                        environment {
                              scannerHome = tool 'sonarscanner4'
                        }
                        steps {
                              withSonarQubeEnv('sonar-pro') {
                              sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=testMaven \
                                    -Dsonar.projectName=A Single Maven Module \
                                    -Dsonar.projectVersion=1.0 \
                                    -Dsonar.sources=maven-samples/single-module/ \
                                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                              }
                              timeout(time: 10, unit: 'MINUTES') {
                                    waitForQualityGate abortPipeline: true
                              }
                        }
            }
      }
}