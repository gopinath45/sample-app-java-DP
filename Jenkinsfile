pipeline{
    agent{
        label "linux"
    }
    environment {
        mvnHome = tool "MAVEN3"
        }
    stages{
        
        stage("Code Quality Scan"){
            steps {
              withSonarQubeEnv('Sonarqube') {
                  sh "${mvnHome}/bin/mvn -f pom.xml sonar:sonar"
                }
            }
        }

        stage("Quality Gate"){
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
                }
            }
        }
        
        stage("Build"){
            steps {
                sh "${mvnHome}/bin/mvn -f pom.xml clean install"
            }
            post {
                success {
                    jacoco()
                }
            }
        }

        stage("Upload Artifacts to Nexus"){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'my-app', classifier: '', file: 'target/my-app-3.0-SNAPSHOT.jar', type: 'jar']], credentialsId: 'nexus', groupId: 'com.mycompany.app', nexusUrl: '192.168.29.70:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '3.0-SNAPSHOT'
            }
       
        }

    }
    
}
