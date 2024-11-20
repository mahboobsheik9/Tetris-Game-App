# Deploy-Tetris-Game-to-Azure-App-Service-using-Azure-Pipelines

- This project will divide in 2 parts. i.e: Build pipeline & Release pipeline and in Build Pipeline if developers makes any change in git hub repository it will trigger the build pipeline and pulls the code from git hub repository and build the docker container image and push it to the Azure container registry
- Release Pipeline will be automatically gettriggered and it will pull the artifact from the build pipelinewhich is an image in ACR and this image it will deploy on the Azure Container App on the Azure App Services and hence will able to see the changes on the gaming application.

Prerequisites :
- An Azure account with an active subscription. <a href="https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F" target="_blank">Create an account for free.</a> 
- An Azure DevOps organization. <a href="https://learn.microsoft.com/en-us/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops" target="_blank">Create an account for free.</a>
- An approval for parallelism to run the pipelines in the Azure Devops Organisations.
- A Github account and all the application files in your local device.
  
Steps to deploy and host the Application using Azure Web App and Azure CI/CD Pipelines:

1. Goto Azure Portal and first create a Resource Group in a specific region of your choice. 
2. Goto >> Search >> Container Registries >> and then create a Azure Container Registry by passing all the values like Resource group, Registry name, Location, Service Plan . If your looking for minimal cost or a cost efficient plan then go for 'Standard'.
3. After that goto your Azure Devops organisations and create a new Azure Devops project and set the visibility as 'Private'.
4. Then goto your Project Settings >> Service Connections >> select 'Docker Registry' >> select 'Azure Container Registry' >> set authentication type as 'Service Principal'.
5. A window gets popped up automatically and there you can authenticate your Azure subscription by passing your credentials like username and password then select your 'Azure Container Registry' and provide your 'Service connection name'. Select the checkbox to grant access for all pipelines and click on Save. This will setup the connection between your Azure Container Registry and your Azure Devops portal.
6. Goto your Github account and create a new repository with name 'Tetris-Game-App' or any name related to your application and upload all the project files and folders, commit changes into your Github repository. 
7. Goto Azure Devops portal >> Service Connections >> select Github >> under OAuth configuration select Azure Pipelines . Here your Github account gets linked to the Azure Devops portal and loads the Service connection name automatically. Select the below checkbox to grant access permission for all pipelines and click on Save

Creating Build Pipeline (CI):

8. Goto Pipelines in the Azure Devops portal >> create new pipeline >> select Github Yaml >> select your Git Repository >> select Docker with build and push image to the container registry option . Authenticate your Azure account and select your Container Registry. click on Validate and configure and the yaml file is displayed then click Save and Run. Add your commit message or leave as default and click Save and Run again. Here you have done the configuration to create the build for your pipeline in few mins build will get created.
9. Once the Build gets succeded goto Azure portal and open your Container Registry >> Services >> Repositories . Here you can see the tetrisgameapp image inside the repository which is pushed once the build gets created.
10. Goto Settings >> Access and keys >> Enable Admin User . This is required for us in the next step for creating a Web App in Azure.

Creating Azure Web App for Release Pipeline (CD):

11. Goto Azure portal >> Search App service >> select Web App and fill the details line Subscription, RG, Name of Web App, Region, Publish as Container, Operating system and Linux plan as default, Pricing plan as Basic B1 for cost efficiency. Click on Database >> we are not using database here so skip it >> click on Container >>  select Image source as Azure Container registry. Here you are connecting your Container registry and to your Web App.
12. Click next: Networking(leave as default) >> Click next: Monitor and secure(leave as default) >>Click next: Tags(optional) >> Click on Review and create and click on Create and finally the Azure Web App will get created.

Designing Release Pipeline (CD):

13. Goto Azure Devops portal >> Pipelines >> Releases >> Create >> select Azure App service deployment >> click on add Artifact >> Under My Project >> Source >> select your build pipeline and click Add >> click on CD icon upon Artifact >> Enable Continuous deployment trigger
14. In the next step you need to configure Stages. Goto Stages >> click on 1 job, 1 task >> Authorize your Azure subscription >> select App type as Web App for Containers(Linux) >> Select App service name (name given for Azure Web App) >> Add Registry name in the format(containerregistryname.azurecr.io) >> Repository - add name of repository inside container registry >> click on Save and click OK
15. Click on Create Release >> add Release description >> click Create . Hence the release pipeline is created . Click on Release-1 at the top and under Stages you can see the pipeline has started. Wait until it is completed.
16. Goto Azure portal >> App Services >> Open your Web App >> Overview >> copy the default domain url and paste it in your web browser and we can see the application is hosted on top of Azure App Services and we can play the game in the browser.






