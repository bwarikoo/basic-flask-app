pipeline {
    agent any

    stages {
        stage('Code Checkout') {
            steps {
                git url: "https://github.com/bwarikoo/basic-flask-app.git",
                branch: "master"
            }
        }
        stage('Run SAST') {
            steps {
               sh "bandit -r $WORKSPACE -f json -o bandit_report.json"
            }
        }
    }
    post {
        always {
            // Archive the Bandit report as an artifact
            archiveArtifacts 'bandit_report.json'

            // Publish the Bandit report as an HTML report
            publishHTML(target: [
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '', // Use the default workspace
                reportFiles: 'bandit_report.json',
                reportName: 'Bandit Report',
                reportTitles: 'Bandit Security Scan'
            ])
        }
    }
}
