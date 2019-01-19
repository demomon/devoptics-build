pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo "Producing: devoptics-build-${BUILD_NUMBER}"
        devOpticsProduces file: '', id: "devoptics-build-${BUILD_NUMBER}", label: '', type: 'demo'
      }
    }
  }
}
