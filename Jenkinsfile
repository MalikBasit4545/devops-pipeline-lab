pipeline {
    agent any

    environment {
        GMAIL_USER = credentials('gmail-username')  // From Jenkins credentials
        GMAIL_PASS = credentials('gmail-password')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/YOUR_USERNAME/devops-pipeline-lab.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'  // or pip install -r requirements.txt, etc.
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'  // Replace with actual test command
            }
        }

        stage('Static Code Analysis') {
            steps {
                sh 'npx eslint .'
            }
        }

        stage('Code Coverage Check') {
            steps {
                script {
                    def coverage = sh(script: "node coverage-check.js", returnStdout: true).trim()
                    if (coverage.toInteger() < 80) {
                        error "Code coverage below 80%"
                    }
                }
            }
        }

        stage('Archive Artifacts') {
            when {
                expression {
                    return env.GIT_TAG == 'release'
                }
            }
            steps {
                archiveArtifacts artifacts: '**/build/**', fingerprint: true
            }
        }
    }

    post {
        success {
            mail to: 'team@example.com',
                 subject: "Build Success - ${env.JOB_NAME}",
                 body: "The Jenkins build ${env.BUILD_NUMBER} was successful.",
                 from: "${env.GMAIL_USER}"
        }

        failure {
            mail to: 'team@example.com',
                 subject: "Build Failed - ${env.JOB_NAME}",
                 body: "The Jenkins build ${env.BUILD_NUMBER} failed.",
                 from: "${env.GMAIL_USER}"
        }
    }
}
