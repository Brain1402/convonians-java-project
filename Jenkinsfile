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
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcatpassword', path: '', url: 'http://52.91.103.249:8080/')], contextPath: 'myapp', war: '**/*.war'
            }
        }
    }
}
