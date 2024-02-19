pipeline {
    agent { label 'docker-agent-alpine' }
    tools {
        gradle 'Gradle-8.6'
    }
    stages {
        stage('Build java project') {
            steps {
                sh '''
                    ./gradlew build
                    java -jar build/libs/JenKins-Intro.jar
                '''
            }
        }
    }
}
