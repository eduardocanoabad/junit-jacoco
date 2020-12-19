pipeline {
    options {
        disableConcurrentBuilds()
    }
    
    agent any

    environment {
        BUILD_URL = "${BUILD_URL}"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building project...'
                sh 'cd java-junit-code-coverage-jacoco-gradle' 
                sh './gradlew install'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh './gradlew test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy project...'
            }
        }
        }
        post {
            always {
                influxDbPublisher(selectedTarget: 'TestDB', customData: assignURL(BUILD_URL))
            }
        }
    }

def assignURL(build_url) {
    def buildURL = [:]
    buildURL['url'] = build_url
    return buildURL
}
