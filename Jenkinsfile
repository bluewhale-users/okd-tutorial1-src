library identifier: "pipeline-library@v1.5",
retriever: modernSCM(
  [
    $class: "GitSCMSource",
    remote: "https://github.com/redhat-cop/pipeline-library.git"
  ]
)

// The name you want to give your Spring Boot application
// Each resource related to your app will be given this name
appName = "testblog"

pipeline {
  agent {
    node {
      label 'nodejs' 
    }
  }
  options {
    timeout(time: 20, unit: 'MINUTES') 
  }
  stages {
    stage("Checkout") {
        steps {
            checkout scm
        }
    }
    stage('Build') {
      steps {
          sh 'npm install'
          sh 'CI=false npm run build'
      }
    }    
    stage("Docker Build") {
        steps {
            // This uploads your application's source code and performs a binary build in OpenShift
            // This is a step defined in the shared library (see the top for the URL)
            // (Or you could invoke this step using 'oc' commands!)           
            binaryBuild(buildConfigName: appName, buildFromPath: ".")
        }
    }
  }
}