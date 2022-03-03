pipeline {
  agent any
  environment {
      MAJOR = '1'
      MINOR = '0'
  }
  stages {
    stage ('Build') {
      steps {
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
      }
    }
    stage ('Deploy') {
      steps {
        UiPathDeploy (
          credentials: UserPass('uipath-admin'), 
          entryPointPaths: 'Main.xaml', 
          folderName: 'Shared', 
          orchestratorAddress: 'https://10.41.11.194', 
          orchestratorTenant: 'Default', 
          packagePath: '/var/jenkins_home/UiPathDemoReply/_out/${env.BUILD_NUMBER}', 
          traceLevel: 'Information',
          environments: ''
        )
      }
    }
  }
}
