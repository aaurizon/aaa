pipeline {
    agent any
    tools {
        docker 'docker'
    }

    environment {
        TEST = 'A'
    }

    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }
        stage('Stage 2') {
            steps {
                echo 'Bye!'
            }
        }
        stage('Stage 3 Condition') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                echo 'Bye!'
            }
        }
        stage('Stage 4 Kube') {
            agent {
                docker {
                    image 'bitnami/kubectl:latest'
                }
            }
            environment {
                KUBECONFIG = credentials('kubeconfig')
            }
            steps {
                sh('kubectl version')
                sh('kubectl get pods')
                sh('kubectl --kubeconfig="$KUBECONFIG" get pods')
            }
        }
    }
}
