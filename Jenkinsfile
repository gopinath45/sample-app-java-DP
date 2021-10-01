pipeline{
    agent{
        label "linux"
    }
    environment {
        mvnHome = "MAVEN3"
        }
    stages{
        
        stage("SCM Checkout"){
            steps {
               git clone https://github.com/gopinath45/sample-app-java-DP.git
            }
       
        }

        stage("Code Quality Scan"){
            steps {
              withSonarQubeEnv('My SonarQube Server') {
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
        }

        stage("Upload Artifacts to Nexus"){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'my-app', classifier: '', file: 'target/my-app-3.0-SNAPSHOT.war', type: 'war']], credentialsId: 'nexus', groupId: 'com.mycompany.app', nexusUrl: '192.168.29.70:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '3.0-SNAPSHOT'
            }
       
        }

    }
    
}