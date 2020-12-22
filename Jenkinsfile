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
                sh 'chmod +x java-junit-code-coverage-jacoco-gradle/gradlew'
                sh 'cd java-junit-code-coverage-jacoco-gradle && ls && ./gradlew build' 
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'cd java-junit-code-coverage-jacoco-gradle && ./gradlew test'
                jacoco runAlways: true
            }
        }
        stage('Sonar') {
            environment {
                scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
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