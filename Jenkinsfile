pipeline {
    agent {
        docker {
            image 'maven:latest' 
            args '-v /root/.m2:/root/.m2 -u root'
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
    }
}