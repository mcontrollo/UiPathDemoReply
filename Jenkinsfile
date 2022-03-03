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
              outputPath: "/var/jenkins_home/UiPathDemoReply/_out/${env.BUILD_NUMBER}",
              projectJsonPath: "/var/jenkins_home/UiPathDemoReply/project.json",
              version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
              useOrchestrator: false,
              traceLevel: "Information",
              orchestratorAddress: "https://10.41.11.194",
              orchestratorTenant: "Default",
              credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: “uipath-admin”]
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
                credentials: UserPass('uipath-admin'), 
                entryPointPaths: 'Main.xaml', 
                folderName: 'Shared', 
                orchestratorAddress: 'https://10.41.11.194', 
                orchestratorTenant: 'Default', 
                packagePath: '${WORKSPACE}\\_out\\${env.BUILD_NUMBER}', 
                traceLevel: 'Information',
                environments: ''
              )
          } catch (err) {
              echo err.getMessage()
          }
        }
      }
      stage ('Deploy 2') {
        steps {
          script {
            try {
              UiPathDeploy (
                credentials: UserPass('uipath-admin'), 
                entryPointPaths: 'Main.xaml', 
                folderName: 'Shared', 
                orchestratorAddress: 'https://10.41.11.194', 
                orchestratorTenant: 'Default', 
                packagePath: 'this_path/does_not/exist/${env.BUILD_NUMBER}', 
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
}
