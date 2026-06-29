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

        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck(
                    odcInstallation: 'DependencyCheck',
                    additionalArguments: """
                        --scan .
                        --format HTML
                        --format XML
                        --nvdApiKey ${NVD_API_KEY}
                    """
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

    post {

        always {
            archiveArtifacts artifacts: '**/dependency-check-report.*', fingerprint: true
        }

        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
