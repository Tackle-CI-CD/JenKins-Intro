pipeline {
    agent any
    tools {
        gradle 'Gradle-8.6'
    }
    stages {
        stage('Build java project') {
            steps {
                sh 'gradle -v'
            }
        }
    }
}
