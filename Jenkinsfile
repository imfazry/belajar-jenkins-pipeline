pipeline {
    agent any

    environment {
        def DEV_DB_PASSWORD = "abdnsadsasda"
        // def CICD_YML_REPO = "https://github.com/imfazry/belajar-jenkins-pipeline.git"
        // def GIT_CRED = "Git-fazry"
        // // def SONAR_URL = "https://sonarqube.danamon.co.id/"
        // // def SONAR_LOGIN = "12c1a5ad214cdd46644f1a3f3f837a7251bd2c40"
        // // def SONATYPE_CRED = 'Sonatype'
        // // def ENV_ID = getEnvironmentID(env.GIT_BRANCH)
        // def PROJECT_NAME = getProject(env.GIT_URL)
        // // def NEXUS_UAT_REGISTRY = "nexus-uat.danamon.co.id:8083"
        // def status_job = "SUCCESS"
    }

    stages {
        stage('Build') {
            steps {
                echo("Stage untuk Build")
                echo( ${DEV_DB_PASSWORD} )
                script {

                      sh 'env > env.txt'
                    readFile('env.txt').split("\r?\n").each {
                        println it
                    }

                    if (env.GIT_BRANCH.contains('PR-')) {        // Check if the branch is from Pull Request or not
                        env.BRANCH = env.CHANGE_BRANCH           // var CHANGE_BRANCH is the source branch of the PR
                    } else {
                        env.BRANCH = env.GIT_BRANCH
                    }

                    CICD_YML = readYml2(PROJECT_NAME, CICD_YML_REPO)

                    //
                    // Setup Build Variables
                    //
                    BUILD_BRANCH = ''
                    if ( CICD_YML.build.branch.contains(env.BRANCH) ) {
                        echo 'Found matching build branch'
                        BUILD_BRANCH = BRANCH
                    } else if ( CICD_YML.build.branch.contains('default') ){
                        echo 'No matching build branch found, using default settings'
                        BUILD_BRANCH = 'default'
                    }
                    echo "BUILD_BRANCH = " + BUILD_BRANCH

                    for ( def i=0 ; i<CICD_YML.build.branch.size(); i++ )
                    {
                        if ( CICD_YML.build.branch[i] == BUILD_BRANCH ) {
                            TOOLS = CICD_YML.build.tools[i]
                            BUILD_DIR = CICD_YML.build.directory[i]    // Mandatory
                            BUILD_CMD = CICD_YML.build.command[i]    // Not Mandatory for java
                        }
                    }

                    //
                    // Setup Publish Variables
                    //
                    TARGET_DIR = CICD_YML.publish.directory[0]
                    TYPE = CICD_YML.publish.type[0]
                    if (TYPE == "jar") {
                        pom = readMavenPom file: "${BUILD_DIR}/pom.xml"
                        ARTIFACTID = pom.artifactId
                        VERSION = pom.version
                        GROUPID = pom.groupId
                        currentBuild.displayName  = "#${BUILD_NUMBER}, ${VERSION}"
                    } else if (TYPE == "artifact") {
                        VERSION = 'latest'
                        currentBuild.displayName  = "#${BUILD_NUMBER}, ${VERSION}"
                    }
                    LIB = CICD_YML.publish.library[0]
                    if (LIB == null) LIB = false


                 }
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