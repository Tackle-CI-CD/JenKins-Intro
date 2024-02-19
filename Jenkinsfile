pipeline {
    agent any
    tools {
        gradle 'gradle-8.6'
    }
    stages {
        stage('Build java project') {
            steps {
                sh 'gradle -v'
            }
        }
    }
}
