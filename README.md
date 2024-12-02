# AzureXperience

```
trigger:
  - main

stages:
  - stage: Build
    jobs:
      - job: Build
        pool: SelfHostedPool
        steps:
          - task: Npm@1
            inputs:
              command: 'install'
          - task: Npm@1
            inputs:
              command: 'custom'
              customCommand: 'run build'
          - task: PublishBuildArtifacts@1
            inputs:
              PathtoPublish: 'build'
              ArtifactName: 'drop'
              publishLocation: 'Container'
  
  - stage: Deploy
    jobs:
      - job: Deploy
        pool: SelfHostedPool
        steps:
        - task: DownloadBuildArtifacts@1
          inputs:
            buildType: 'current'
            downloadType: 'single'
            artifactName: 'drop'
            downloadPath: '$(System.ArtifactsDirectory)'
        - task: AzureRmWebAppDeployment@4
          inputs:
            ConnectionType: 'AzureRM'
            azureSubscription: 'Azure for Students (d1bc7cf6-c398-4aeb-898c-d187c2787ac7)'
            appType: 'webAppLinux'
            WebAppName: 'azure-xperience'
            packageForLinux: '$(System.ArtifactsDirectory)/drop'
            RuntimeStack: 'STATICSITE|1.0'
```


![alt text](image.png)