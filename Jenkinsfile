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
                echo "${status_job}"
            }

        }
        
    }
}