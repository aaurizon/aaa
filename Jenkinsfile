pipeline {
    agent any

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
        stage('Stage 2A') {
            agent { label 'testlabels' }
            steps {
                echo 'Bye!'
            }
        }
        stage('Stage 2B') {
            agent { label 'docker' }
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
                kubernetes {
                  //image 'bitnami/kubectl:latest'
                  //containerTemplate {
                  //    name 'kubectl'
                  //    image 'bitnami/kubectl:latest'
                  //    ttyEnabled true
                  //}
                    yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-value
spec:
  containers:
  - name: kubectl
    image: bitnami/kubectl:latest
    command:
    - cat
    tty: true
    securityContext:
      runAsUser: 0
"""
                }
            }
            //environment {
            //    KUBECONFIG = credentials('kubeconfig')
            //}
            steps {
                container('kubectl') {
                    timeout(time: 20, unit: 'SECONDS') {
                        echo 'test'
                        sh 'echo test'
                        sh 'kubectl version --client'
                        sh 'kubectl version'
                        sh 'kubectl get pods'
                        sh 'kubectl get pods -A'
                        //sh 'which kubectl'
                        //sh 'ls /usr/local/bin/kubectl'
                    }
                }
                //sh('kubectl version')
                //sh('kubectl get pods')
                //sh('kubectl --kubeconfig="$KUBECONFIG" get pods')
            }
        }
    }
}
