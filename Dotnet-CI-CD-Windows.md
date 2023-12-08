# Creating a CI/CD pipeline (Yaml) to deploy Asp dotnet core code On windows IIS Server.


| Revision | Change | Date | Author | Approver |
| --- | --- | --- | --- | --- |
| 0.1 | Initial Release |  | Abhishek S. | Shoaib S. |

---

## Objective

We will look how to host Asp dotnet core application on windows server by building and deploying code using Azure Devops pipeline  (Yaml).

## Overview and  imp services
- <ins>Asp dotnet core</ins>: ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based, and connected applications. It's a redesign and rewrite of the original ASP.NET, providing a lightweight and modular framework suitable for a wide range of applications.
- <ins>Important file</ins> : .csproj file it consists of the code version and also it includes the dependencies the code require

  
## Resources or Services to be created in Azure

- Azure Resource Group
- Azure VNET 
- Azure Windows VM (2019)
  



#### Azure Devops
- Overview: Azure Devops is CI/CD Tool or platform for continuous integration and deployment/delivery of code or application on various cloud services or on premises.

#### Steps to create CI pipeline for Asp Dotnet Application
- Creating a repo and pushing code on it :  Goto Azure Devops portal > Click on organization > New project
![](__assests__/snap1.png)
![](__assests__/snap2.png)
- Pushing repo: After creating the repo click on repo and you will sets of git command to push code on Azure repo you created.
  ![](__assests__/snap3.png)
- Creation of PAT Token: While pushing code you will require PAT token for authorization GoTo User Settings (Upper right corner) and select personal access token 
![](__assests__/snap4.png)
![](__assests__/snap5.png)
![](__assests__/snap6.png)

- Initialize git and use command and Token to push code in Azure repo.
- Below Img shows the code which is pushed and also click on setup build on Upper right corner for Creation of CI pipeline.
![](__assests__/snap7.png)

- Creation of CI pipeline: Click on New Pipeline > Select Azure repo where your code is available and in configure select ASP.NET Core
![](__assests__/snap8.png)
Below image shows your basic pipeline looks.

![](__assests__/snap9.png)

Add dotnet restore ensuring that all the necessary NuGet packages, declared as dependencies in your .NET project, are downloaded by reading .csproj file


![](__assests__/snap10.png)

Also catch publish directory or artifact that it generates by choosing task publish build artifact 


![](__assests__/snap11.png)


![](__assests__/snap12.png)
 ### IMP in publish build use $(Build.SourcesDirectory) or output path in console of build given below For eg full path  PathtoPublish: '/home/vsts/work/1/s/bin/Release/net6.0/' 

 ![](__assests__/snap13.png)

 - So my build artifact is store for CD

 ![](__assests__/snap14.png)


 ```
 
trigger:
- master

pool:
  vmImage: ubuntu-latest

variables:
  buildConfiguration: 'Release'

steps:
- script: dotnet restore
  displayName: 'Restore NuGet packages'
- script: dotnet build --configuration $(buildConfiguration)
  displayName: 'dotnet build $(buildConfiguration)'

- task: PublishBuildArtifacts@1
  inputs:
      PathtoPublish: '/home/vsts/work/1/s/bin/Release/net6.0/'
      ArtifactName: 'drop'
      publishLocation: 'Container'




```


#### Steps to create CD pipeline for Asp Dotnet Application

- Click on Release > Add an Artifact > Build as Source Type 
### Imp Drop Down and Tab
- Project: Name of the project that contains the build pipeline. i.e is project
- Source (build pipeline): Name or ID of the build pipeline that publishes the artifact i.e Build number or Tag or ID
- Default version: Means select the build number you can select latest version or specific build bumber that will be use in release pipeline.



![](__assests__/snap15.png)


- Click Add a Stage and Select IIS website deployment.

![](__assests__/snap16.png)

- Click on the IIS deployment and configure Deployment group by clicking on the settings. Deplyment group where your artifact or code deployed for hosting.

![](__assests__/snap17.png)
Above is the script should run on the windows powershell cmd

[Watch the video for Deployment group in windows server from 12 min onwards](https://www.youtube.com/watch?v=BpCwQp-ABcM&t=473s)

- After running script on windows power Shell Deployments groups Targets Shows healthy

![](__assests__/snap18.png)

- Select Deployment group in the release pipeline a
![](__assests__/snap19.png)


![](__assests__/snap20.png)
In above image click on enable iis to install server in windows automatically


- Physical path give the path were your code will be deployed in that particular directory if you add extra folder name it will automatically be created on the remote windows server.

![](__assests__/snap21.png)

- In above image particularly in package or folder section click on the drop down and select the full path of the artifact.

- Because of the incomplete path in the iis web app deploy i have got this issue

![](__assests__/snap22.png)

![](__assests__/snap23.png)

#### Steps to configure windows server

- Create windows 2019 server on Azure and click on RDP file and add username password in Remina and enter into window server

- Install IIS Server

  [Install IIS Server in windows server](https://www.youtube.com/watch?v=eeBm2H1Yuok)

- install git for locally testing of code

    [git installation](https://www.youtube.com/watch?v=cJTXh7g-uCM)

- If error comes in downlaoding software from internet

    [Internet setting for downloading software](https://www.youtube.com/watch?v=7BrMddeIVCA)

- Download rewrite module 

    [Install rewrite module](https://www.youtube.com/watch?v=a5U9Iv_7Vog)

- For Internal server Error

    [500 internal server error](https://www.youtube.com/watch?v=moqe9mh3j2c)




