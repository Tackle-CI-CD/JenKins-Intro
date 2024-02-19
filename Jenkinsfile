pipeline {
    agent any
    stages {
        stage("Build java project") {
            steps {
                echo 'Builing..'
                withGradle() {
                    sh './gradlew build'
                }
            }
        }
    }
}