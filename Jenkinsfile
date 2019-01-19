pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        devOpticsProduces file: '', id: "devoptics-build-${BUILD_NUMBER}", label: '', type: 'demo'
      }
    }
  }
}
