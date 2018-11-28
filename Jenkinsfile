pipeline {
    agent {
        docker {
            image 'maven:latest' 
            args '-v maven-repo:/root/.m2'
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