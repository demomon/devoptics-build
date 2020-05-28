pipeline {
  agent {
    kubernetes {
    label 'mypod'
    yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3-jdk-11-slim
    command: ['cat']
    tty: true
"""
    }
  }
  stages {
    stage('Checkout') {
      steps{
        git 'https://github.com/joostvdg/jx-maven-app.git'
      }
    }
    stage('Build') {
      steps {
        // https://docs.cloudbees.com/docs/cloudbees-devoptics/latest/user-guide/value-streams#withmaven-pipeline-step
        // To use this feature, Jenkins must have both the DevOptics plugin and the Pipeline Maven plugin installed.
        container('maven') {
          script {
            withMaven() {
              sh "mvn clean install"
            }
          }
        }
      }
    }
    stage('DevOptics Sync') {
      steps {
        echo "Producing: devoptics-build-${env.BUILD_ID}"
        gateProducesArtifact file: '', id: "devoptics-build-${env.BUILD_ID}", label: '', type: 'demo'
        build job: 'devoptics/devoptics-deploy/master', parameters: [run(description: 'Run for Build', name: 'RUN', runId: "devoptics/devoptics-build/master#${env.BUILD_ID}")]
      }
    }
  }
}
