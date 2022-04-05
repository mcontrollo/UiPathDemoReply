pipeline {
  agent any
  environment {
      MAJOR = '1'
      MINOR = '0'
  }
  stages {
    stage ('Build') {
      steps {
        script {
          try {
            UiPathPack (
              outputPath: "${WORKSPACE}\\out\\${env.BUILD_NUMBER}",
              projectJsonPath: "${WORKSPACE}\\project.json",
              version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
              useOrchestrator: false,
              traceLevel: "Information",
              orchestratorAddress: 'https://10.41.11.194',
              orchestratorTenant: "Default",
              credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "uipath-admin"]
            )
          } catch (err) {
              echo err.getMessage()
          }
        }
      } 
    }
    stage ('Deploy') {
      steps {
        script {
          try {
              UiPathDeploy (
                credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "uipath-admin"], 
                entryPointPaths: 'Main.xaml', 
                folderName: 'Shared', 
                orchestratorAddress: 'https://10.41.11.194', 
                orchestratorTenant: 'Default', 
                packagePath: "${WORKSPACE}\\out\\${env.BUILD_NUMBER}", 
                traceLevel: 'Information',
                environments: ''
              )
          } catch (err) {
              echo err.getMessage()
          }
        }
      }
    }
  }
  post {
    always {
        echo "Package built stored in ${WORKSPACE}\\out\\${env.BUILD_NUMBER}"
    }
  }
}
