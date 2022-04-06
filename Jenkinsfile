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
credentials: ExternalApp(accountForApp: 'Jenkins', applicationId: '483c8948-76ee-4016-9695-7b7ec4ff3ad1', applicationScope: 'OR.Webhooks.Read OR.Webhooks.Write OR.Monitoring OR.Monitoring.Read OR.Monitoring.Write OR.ML OR.ML.Read OR.ML.Write OR.Tasks OR.Tasks.Read OR.Tasks.Write OR.Analytics OR.Analytics.Read OR.Analytics.Write OR.Webhooks OR.Folders OR.Folders.Write OR.BackgroundTasks OR.BackgroundTasks.Read OR.BackgroundTasks.Write OR.TestSets OR.TestSets.Read OR.TestSets.Write OR.TestSetExecutions OR.TestSetExecutions.Read OR.TestSetExecutions.Write OR.TestSetSchedules OR.TestSetSchedules.Read OR.TestSetSchedules.Write OR.TestDataQueues OR.Folders.Read OR.Audit.Write OR.Audit.Read OR.Audit OR.License OR.License.Read OR.License.Write OR.Settings OR.Settings.Read OR.Settings.Write OR.Robots OR.Robots.Read OR.Robots.Write OR.Machines OR.Machines.Read OR.Machines.Write OR.Execution OR.Execution.Read OR.Execution.Write OR.Assets OR.Administration.Write OR.Administration.Read OR.Administration OR.Users.Write OR.Users.Read OR.Users OR.TestDataQueues.Read OR.Jobs.Write OR.Jobs OR.Queues.Write OR.Queues.Read OR.Queues OR.Assets.Write OR.Assets.Read OR.Jobs.Read OR.TestDataQueues.Write OR.Hypervisor.Write OR.Hypervisor.Read OR.Hypervisor', applicationSecret: 'uipath-cloud-app-secret', identityUrl: ''), entryPointPaths: 'Main.xaml', environments: '', folderName: 'Shared', orchestratorAddress: 'https://cloud.uipath.com/', orchestratorTenant: 'DefaultTenant', packagePath: '${WORKSPACE}\\out\\${env.BUILD_NUMBER}', traceLevel: 'Information' 
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
