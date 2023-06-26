pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        }
        stage('Build') {
            steps {
                sh 'cd SampleWebApp && mvn clean package'
            }
        }
        
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sonarscanner') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'sampleWebApp', classifier: '', file: 'SampleWebApp/target/SamplewebApp.war', type: 'WAR']], credentialsId: 'nexuspassword', groupId: 'sampleWebApp', nexusUrl: 'ec2-44-202-229-74.compute-1.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcatpassword', path: '', url: 'http://52.91.103.249:8080/')], contextPath: 'myapp', war: '**/*.war'
            }
        }
    }
}
