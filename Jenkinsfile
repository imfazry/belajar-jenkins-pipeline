def status_job = "SUCCESS"
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo("Stage untuk Build")
            }
        }
        stage('Test'){
            steps {
                echo("Stage untuk Testing")
            }
        }
        stage('Delploy'){
            steps {
                echo("Stage untuk Deploy")
                echo "${BUILD_NUMBER}"
                echo "${JOB_NAME}"
                echo "${NODE_NAME}"
            }
        }
        stage('Email notification'){
            steps {
                echo "Email notification"
                echo "${status_job}"
                
            }

        }
        post {
            failure {
                script {
                    currentBuild.result = 'FAILURE'
                    emailext (
                        to: 'muhamad.fazry93@gmail.com',
                        subject: "Pipeline Failed: ${currentBuild.fullDisplayName}",
                        body: "The pipeline ${currentBuild.fullDisplayName} has failed.\n\nBuild URL: ${env.BUILD_URL}"
                        )
                    }
                }
            success {
                script {
                    currentBuild.result = 'SUCCESS'
                    emailext (
                        to: 'muhamad.fazry93@gmail.com',
                    subject: "Pipeline Succeeded: ${currentBuild.fullDisplayName}",
                    body: "The pipeline ${currentBuild.fullDisplayName} has succeeded.\n\nBuild URL: ${env.BUILD_URL}"
                )
                }
            }
        }
    }
} 