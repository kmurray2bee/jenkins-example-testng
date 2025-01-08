pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    purpose: testng-demo
spec:
  containers:
    - name: maven
      image: maven:3.8.8-openjdk-11
      command:
        - cat
      tty: true
      volumeMounts:
        - name: maven-cache
          mountPath: /root/.m2
  volumes:
    - name: maven-cache
      emptyDir: {}
"""
        }
    }
    stages {
        stage('Run the tests') {
            steps {
                container('maven') {
                    sh './mvnw clean test'
                }
            }
        }
    }
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
