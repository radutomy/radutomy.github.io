---	
layout: post	
title: Real Time Page Updates in Blazor (P2)
image: /img/p3/header.png
---	

## This is a 3 part series:
- [Part I convers building the Blazor client and demonstrating how it can be used to process events in real time](https://radutomy.github.io/2020-05-08-realtimeblazor-p1/)
- [In Part II  we demonstrate how we can leverage Azure and Github Pages in order to build a complete CI pipeline that builds and deploys the application automatically](https://radutomy.github.io/2020-05-15-realtimeblazor-p2/)
- Finally, in part III we're adding covering unit testing via Azure

## Introduction

In the previous post we built a Blazor Web Assembly client application that displays the price for a basket of cryptocurrencies in real time. In this post we'll learn how to build the repository using Azure and deploy the page to Github Pages automatically.  

We'll assume you have an Azure dev account and the repository is hosted on GitHub.

Head over to Azure dashboard, click on Pipelines -> New Pipeline

![alt text][1]

Select the repository where the Blazor app is hosted and then continue. On the next page you'll be asked to chose your pipeline configuration. Select ASP.NET Core.

![alt text][2]

In the next page we need to configure the YAML that instructs the Azure VM how to build our repository.

The schematic is simple and follows this flow:

`Copy from the repository` -> `Build the source code` -> `Publish the app artifacts` -> `Commit artifacts to GitHub Pages`

```yml
# ASP.NET Core
# Build and test ASP.NET Core projects targeting .NET Core.
# Add steps that run tests, create a NuGet package, deploy, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/dotnet-core

trigger:
- master

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'

steps:
- script: dotnet build --configuration $(buildConfiguration)
  displayName: 'dotnet build $(buildConfiguration)'

- task: DotNetCoreCLI@2
  displayName: "Publish App"
  inputs:
    command: 'publish'
    arguments: '--configuration $(BuildConfiguration) --output $(Build.ArtifactStagingDirectory)'
    zipAfterPublish: false

- task: PublishBuildArtifacts@1
  displayName: "Upload Artifacts"
  inputs:
    pathtoPublish: '$(Build.ArtifactStagingDirectory)'
    artifactName: 'dist'

- script: |
    git config --global user.name "$GITHUBNAME"
    git config --global user.email "GITHUBEMAIL"
    cd $(Build.ArtifactStagingDirectory)/src/wwwroot
    touch .nojekyll
    git init
    git checkout -b gh-pages
    git add -A
    git commit -m "Created build"
    git push -f https://$GITHUBPAT@github.com/radutomy/RealTimeBlazor.git gh-pages
  displayName: 'Publish to GitHub Pages'
```


Demo pagee: [https://radutomy.github.io/RealTimeBlazor/](https://radutomy.github.io/RealTimeBlazor/)
Source code: [https://github.com/radutomy/RealTimeBlazor](https://github.com/radutomy/RealTimeBlazor)

[1]: https://raw.githubusercontent.com/radutomy/radutomy.github.io/master/img/p4/img1.JPG
[2]: https://raw.githubusercontent.com/radutomy/radutomy.github.io/master/img/p4/img2.JPG
[3]: https://raw.githubusercontent.com/radutomy/radutomy.github.io/master/img/p4/img3.JPG
[4]: https://raw.githubusercontent.com/radutomy/radutomy.github.io/master/img/p4/img4.JPG