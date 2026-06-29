pipeline {

    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        NVD_API_KEY = credentials('nvd-api-key')
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('OWASP Scan') {
            steps {
                dependencyCheck(
                    odcInstallation: 'DependencyCheck',
                    additionalArguments: '--scan . --format HTML --format XML --nvdApiKey=$NVD_API_KEY --nvdApiDelay 8000 --nvdMaxRetryCount 50'
                )
            }
        }

        stage('Publish Report') {
            steps {
                dependencyCheckPublisher(
                    pattern: '**/dependency-check-report.xml'
                )
            }
        }
    }
}
