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
    stage('Write Settings File') {
      environment {
        TOKEN = credentials('github-package-read')
      }
      steps {
        writeFile encoding: 'UTF-8', file: 'settings.xml', text: """<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      https://maven.apache.org/xsd/settings-1.0.0.xsd">
  
  <servers>
    <server>
      <id>github-jx-maven-lib</id>
      <username>joostvdg</username>
      <password>${TOKEN}</password>
    </server>
  </servers>
</settings>"""
      }
    }
    stage('Build') {
      steps {
        
        // https://docs.cloudbees.com/docs/cloudbees-devoptics/latest/user-guide/value-streams#withmaven-pipeline-step
        // To use this feature, Jenkins must have both the DevOptics plugin and the Pipeline Maven plugin installed.
        container('maven') {
          script {
            withMaven() {
              sh "mvn clean install -s settings.xml"
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
