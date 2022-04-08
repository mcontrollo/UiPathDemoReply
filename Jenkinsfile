pipeline {
  agent any
  environment {
      MAJOR = '1'
      MINOR = '0'
      UIPATH_ORCH_URL = "https://cloud.uipath.com/"
      UIPATH_ORCH_LOGICAL_NAME = "demoobkzieep"
      UIPATH_ORCH_TENANT_NAME = "DefaultTenant"
      UIPATH_ORCH_FOLDER_NAME = "Shared"
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
              useOrchestrator: true,
              traceLevel: "Information",
              orchestratorAddress: "${UIPATH_ORCH_URL}",
              orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
              credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'uipath-cloud-api-access')
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
                credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'uipath-cloud-api-access'), 
                entryPointPaths: 'Main.xaml', 
                environments: '', 
                folderName: "${UIPATH_ORCH_FOLDER_NAME}", 
                orchestratorAddress: "${UIPATH_ORCH_URL}", 
                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}", 
                packagePath: "${WORKSPACE}\\out\\${env.BUILD_NUMBER}", 
                traceLevel: 'Information'
              )
          } catch (err) {
              echo err.getMessage()
          }
        }
      }
    }
  }
  post {
    success {
        echo "Package built stored in ${WORKSPACE}\\out\\${env.BUILD_NUMBER}"
    }
  }
}
