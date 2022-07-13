#!/usr/bin/env groovy
pipeline {
    agent { label "linux && docker" }
    environment {
      AWS_ACCESS_KEY_ID = "${AWS_DEPLOY_USER_CREDENTIALS_USR}"
      AWS_SECRET_ACCESS_KEY = "${AWS_DEPLOY_USER_CREDENTIALS_PSW}"
      CONTAINER_REGISTERY = "${AWS_ACCOUNT_ID}"+".dkr.ecr."+"${AWS_DEFAULT_REGION}"+".amazonaws.com"
      CONTAINER_REPO_NAME = "${APPLICATION_NAME}"
      CONTAINER_IMAGE_TAG = "${GIT_COMMIT}"
      CONTAINER_IMAGE_NAME = "${CONTAINER_REGISTERY}"+"/"+"${CONTAINER_REPO_NAME}"+":"+"${CONTAINER_IMAGE_NAME_TAG}"
      CONTAINER_BUILD_TOOL = "docker"
    }

    options {
        timeout(time: 20, unit: 'MINUTES')
    }

    stages {
        stage('Pre-build') {
            steps {
                sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | ${CONTAINER_BUILD_TOOL} login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
            }
        }
        stage('Build container image') {
            steps {
                sh """
                    ${CONTAINER_BUILD_TOOL} build --rm=true -t ${CONTAINER_IMAGE_NAME} .
                """
            }
        }
        stage('Push container image to registery') {
            steps {
                sh """
                    ${CONTAINER_BUILD_TOOL} push ${CONTAINER_IMAGE_NAME}
                """
            }
        }
        stage("Deploy") {
            steps {
                node ("") {
                    script(){
                        // https://www.jenkins.io/doc/pipeline/steps/ssh-steps/#sshscript-ssh-steps-sshscript-execute-scriptfile-on-remote-node
                        withCredentials([sshUserPrivateKey(credentialsId: 'deploy_user', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
                            def server = [:]
                            server.name = "staging-server"
                            server.host = "x.x.x.x"
                            server.allowAnyHosts = true
                            server.user = userName
                            server.identityFile = identity
                            sshCommand remote: server, command: "aws ecr get-login-password | docker login --username AWS --password-stdin ${CONTAINER_REGISTERY}"
                            sshCommand remote: server, command: "docker pull ${CONTAINER_IMAGE_NAME}"
                            sshCommand remote: server, command: "docker stop driverfly-backend || true"
                            sshCommand remote: server, command: "docker rm driverfly-backend || true"
                            sshCommand remote: server, command: "docker run -d -P --name driverfly-backend --restart always ${CONTAINER_IMAGE_NAME}"
                        }
                    }
                }
            }
        }
    }
    post {
        failure {
            // updateGitlabCommitStatus name: 'build', state: 'failed'
            slackSend(message: "Build has failed.")
            slackSend(message: "${BUILD_URL}console", sendAsText: true)
        }
        success {
            // updateGitlabCommitStatus name: 'build', state: 'success'
            slackSend(message: "Build has run successfully.")
            slackSend(message: "${BUILD_URL}console", sendAsText: true)
        }
        always {
            // updateGitlabCommitStatus name: 'build', state: 'success'
            slackSend(message: "Build has run successfully.")
            slackSend(message: "${BUILD_URL}console", sendAsText: true)
        }
    }
}
