pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo "Producing: devoptics-build-${BUILD_NUMBER}"

	gateProducesArtifact file: '', id: "devoptics-build-${BUILD_NUMBER}", label: '', type: 'demo'
        
        // deprecated as of February 2019
        //devOpticsProduces file: '', id: "devoptics-build-${BUILD_NUMBER}", label: '', type: 'demo'
      }
    }
  }
}
