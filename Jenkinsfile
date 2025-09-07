pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'npm install --legacy-peer-deps'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Mocha tests...'
                sh 'npm test'
            }
            post {
                always {
                    emailext(
                        to: 'vasusund1977@gmail.com',
                        subject: "Test Stage Report - Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                        body: """<p>Test stage finished with status: ${currentBuild.currentResult}</p>
                                 <p>See attached log for details.</p>""",
                        attachLog: true
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running npm audit for vulnerabilities...'
                sh 'npm audit --audit-level=low || true'
            }
            post {
                always {
                    emailext(
                        to: 'vasusund1977@gmail.com',
                        subject: "Security Scan Report - Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                        body: """<p>Security scan finished with status: ${currentBuild.currentResult}</p>
                                 <p>See attached log for details.</p>""",
                        attachLog: true
                    )
                }
            }
        }
    }

    post {
        success {
            emailext(
                to: 'vasusund1977@gmail.com',
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>Pipeline succeeded.</p>""",
                attachLog: true
            )
        }
        failure {
            emailext(
                to: 'vasusund1977@gmail.com',
                subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>Pipeline failed.</p>""",
                attachLog: true
            )
        }
    }
}
