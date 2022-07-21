#!/usr/bin/env groovy
@Library('jenkins-global-pipeline-libraries') _

// Jenkinsfile
VERSION = "0.0.7"

pipeline {
    agent { label "linux && k8s-client" }

    options {
        timestamps()
        timeout(time: 20, unit: 'MINUTES')
        skipStagesAfterUnstable()
        buildDiscarder(logRotator(numToKeepStr:'10'))
    }

    environment {
        AWS_DEPLOY_USER_CREDENTIALS = credentials ('AWS_deploy_user')
        AWS_ACCESS_KEY_ID = "${AWS_DEPLOY_USER_CREDENTIALS_USR}"
        AWS_SECRET_ACCESS_KEY = "${AWS_DEPLOY_USER_CREDENTIALS_PSW}"
        AWS_IMAGE_PULL_SECRET_NAME = "aws-image-pull-secret"
        ENVIRONMENT = "staging"
        KUBECONFIG_DEVELOPMENT = credentials ('KUBECONFIG_DEVELOPMENT')
        KUBECONFIG_STAGING = credentials ('KUBECONFIG_STAGING')
        KUBECONFIG_PRODUCTION = credentials ('KUBECONFIG_PRODUCTION')

        KUBECONFIG_FILE_NAME = GET_KUBECONFIG_FILE_NAME_BASED_ON_ENVIRONMENT("${ENVIRONMENT}")
        KUBE_NAMESPACE = GET_KUBE_NAMESPACE_BASED_ON_ENVIRONMENT("${ENVIRONMENT}")
        CONTAINER_REPO_NAME = "${APPLICATION_NAME}"
        GIT_COMMIT_SHORT = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
        CONTAINER_IMAGE_TAG = "${GIT_COMMIT_SHORT}"
        CONTAINER_IMAGE_NAME = "${CONTAINER_REGISTERY}"+"/"+"${CONTAINER_REPO_NAME}"+":"+"${CONTAINER_IMAGE_TAG}"
        KUBE_DEPLOYMENT_TEMPLATE = credentials ('KUBE_DEPLOYMENT_TEMPLATE')
        KUBE_SERVICE_TEMPLATE = credentials ('KUBE_SERVICE_TEMPLATE');
        KUBE_CONFIGMAP_TEMPLATE = credentials ("KUBE_CONFIGMAP_TEMPLATE_${APPLICATION_NAME}_${ENVIRONMENT}");
        KUBE_INGRESS_TEMPLATE = credentials ('KUBE_INGRESS_TEMPLATE');
        KUBE_HPA_TEMPLATE = credentials ('KUBE_HPA_TEMPLATE');
        KUBE_VPA_TEMPLATE = credentials ('KUBE_VPA_TEMPLATE');
    }

    stages {
        stage ('Env check') {
            steps {
                sh "printenv"
            }
        }
        stage ('Start') {
            steps {
                sendNotifications 'STARTED'
            }
        }
        stage ('SCM') {
            steps {
                script{
                    checkout scm
                }
            }
        }
        stage ('Container repository authentication') {
            steps {
                sh 'aws ecr get-login-password --region $AWS_DEFAULT_REGION | $CONTAINER_BUILD_TOOL login --username AWS --password-stdin $CONTAINER_REGISTERY'
            }
        }
        stage ('Build container image') {
            steps {
                sh """
                    ${env.CONTAINER_BUILD_TOOL} build --rm=true -t ${CONTAINER_IMAGE_NAME} .
                """
            }
        }
        stage ('Push container image to registery') {
            steps {
                sh """
                    ${env.CONTAINER_BUILD_TOOL} push ${CONTAINER_IMAGE_NAME}
                """
            }
        }
        stage ("Deploy to k8s cluster") {
            steps {
                    sh '''
                        #!/usr/bin/env bash
                        set -vx
                        mv $KUBECONFIG_FILE_NAME ~/.kube/config
                        KUBECONFIG="~/.kube/config"

                        TOKEN=$(aws ecr get-login-password --region $AWS_DEFAULT_REGION)
                        kubectl get secrets $AWS_IMAGE_PULL_SECRET_NAME && kubectl --insecure-skip-tls-verify=true delete secret --ignore-not-found $AWS_IMAGE_PULL_SECRET_NAME
                        kubectl get secrets $AWS_IMAGE_PULL_SECRET_NAME || kubectl --insecure-skip-tls-verify=true create secret docker-registry $AWS_IMAGE_PULL_SECRET_NAME \
                        --docker-server=$CONTAINER_REGISTERY \
                        --docker-username=AWS \
                        --docker-password="${TOKEN}" \
                        --docker-email="example@example.com" && kubectl --insecure-skip-tls-verify=true patch serviceaccount default -p '{"imagePullSecrets":[{"name":"'$AWS_IMAGE_PULL_SECRET_NAME'"}]}'

                        # Prepare k8s manifests

                        # Replace variables with values from environment
                        FILENAME="svc.yaml"
                        envsubst < $KUBE_SERVICE_TEMPLATE > $FILENAME
                        # Check if there is a difference between the manifest and the current state of the cluster
                        set +e
                        kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE diff -f $FILENAME
                        SVC_DIFF=$?
                        set -e
                        # Apply manifest
                        kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE --wait=true apply -f $FILENAME

                        # Replace variables with values from environment
                        FILENAME="ingress.yaml"
                        envsubst < $KUBE_INGRESS_TEMPLATE > $FILENAME
                        # Check if there is a difference between the manifest and the current state of the cluster
                        set +e
                        kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE diff -f $FILENAME
                        INGRESS_DIFF=$?
                        set -e
                        # Apply manifest
                        kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE --wait=true apply -f $FILENAME

                        # Replace variables with values from environment
                        FILENAME="configmap.yaml"
                        envsubst < $KUBE_CONFIGMAP_TEMPLATE > $FILENAME
                        # Check if there is a difference between the manifest and the current state of the cluster
                        set +e
                        kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE diff -f $FILENAME
                        CONFIGMAP_DIFF=$?
                        set -e
                        # Apply manifest
                        kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE --wait=true apply -f $FILENAME

                        # Replace variables with values from environment
                        FILENAME="deployment.yaml"
                        envsubst < $KUBE_DEPLOYMENT_TEMPLATE > $FILENAME
                        # Check if there is a difference between the manifest and the current state of the cluster
                        set +e
                        kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE diff -f $FILENAME
                        DEPLOYMENT_DIFF=$?
                        set -e
                        # Apply manifest
                        kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE --wait=true apply -f $FILENAME

                        # Replace variables with values from environment
                        FILENAME="hpa.yaml"
                        envsubst < $KUBE_HPA_TEMPLATE > $FILENAME
                        # Check if there is a difference between the manifest and the current state of the cluster
                        set +e
                        kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE diff -f $FILENAME
                        HPA_DIFF=$?
                        set -e
                        # Apply manifest
                        kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE --wait=true apply -f $FILENAME

                        # # Replace variables with values from environment
                        # FILENAME="vpa.yaml"
                        # envsubst < $KUBE_VPA_TEMPLATE > $FILENAME
                        # # Check if there is a difference between the manifest and the current state of the cluster
                        # set +e
                        # kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE diff -f $FILENAME
                        # VPA_DIFF=$?
                        # set -e
                        # # Apply manifest
                        # kubectl --insecure-skip-tls-verify=true -n $KUBE_NAMESPACE --wait=true apply -f $FILENAME

                        if [ $SVC_DIFF -eq 0 ] && [ $INGRESS_DIFF -eq 0 ] && [ $CONFIGMAP_DIFF -eq 0 ] && [ $DEPLOYMENT_DIFF -eq 0 ]; then
                            echo "No changes detected in the cluster"
                        else
                            echo "Changes detected in the cluster"
                        fi

                        # Check both deployment and configmap changes, trigger a rolling update if there is a change in configmap and no change in deployment.
                        if [ $DEPLOYMENT_DIFF -eq 0 ] && [ $CONFIGMAP_DIFF -ne 0]; then
                            echo "Configmap changed but deployment hasn't. Applying configmap, and trigger a rolling update."
                            kubectl rollout restart deployment/${APPLICATION_NAME}
                        fi

                        # Cleanup: Remove temporary files
                        rm deployment.yaml || true
                        rm svc.yaml || true
                        rm ingress.yaml || true
                        rm configmap.yaml || true
                        rm hpa.yaml || true
                        rm vpa.yaml || true
                    '''
            }
        }
    }
    post {
        always {
            // TODO: Get more detail if there is a failure, we can upload to Slack
            sendNotifications "${currentBuild.result}"
        }
    }
}

def GET_KUBECONFIG_FILE_NAME_BASED_ON_ENVIRONMENT(String ENVIRONMENT) {
    if(ENVIRONMENT.equals("development")) {
        return "$KUBECONFIG_DEVELOPMENT";
    } else if (ENVIRONMENT.equals("staging")) {
        return "$KUBECONFIG_STAGING";
    } else if (ENVIRONMENT.equals("production")) {
        return "$KUBECONFIG_PRODUCTION";
    } else {
        return 0;
    }
}

def GET_KUBE_NAMESPACE_BASED_ON_ENVIRONMENT(String ENVIRONMENT) {
    if(ENVIRONMENT.equals("development")) {
        return "default";
    } else if (ENVIRONMENT.equals("staging")) {
        return "default";
    } else if (ENVIRONMENT.equals("production")) {
        return "default";
    } else {
        return 0;
    }
}
