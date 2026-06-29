pipeline {
    agent any
    tools { maven 'Maven' }
    stages {
        stage('Checkout'){ steps { checkout scm } }
        stage('Build'){ steps { sh 'mvn clean package' } }
        stage('OWASP Scan'){
            steps{
                dependencyCheck additionalArguments: '--scan .',
                                odcInstallation: 'DependencyCheck'
            }
        }
        stage('Publish Report'){
            steps{
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
    }
}
