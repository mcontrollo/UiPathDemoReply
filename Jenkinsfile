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
credentials: Token(accountName: 'demoobkzieep', credentialsId: 'uipath-cloud-api-access'), entryPointPaths: 'Main.xaml', environments: '', folderName: 'Shared', orchestratorAddress: 'https://cloud.uipath.com/', orchestratorTenant: 'DefaultTenant', packagePath: "${WORKSPACE}\\out\\${env.BUILD_NUMBER}", traceLevel: 'Information'
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
