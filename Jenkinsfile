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
    image: debian:stretch
    command:
    - cat
    tty: true
"""
                }
            }
            //environment {
            //    KUBECONFIG = credentials('kubeconfig')
            //}
            steps {
                container('kubectl') {
                    timeout(time: 20, unit: 'SECONDS') {
                        //sh 'kubectl version --client'
                        echo 'test'
                        sh 'echo test'
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
