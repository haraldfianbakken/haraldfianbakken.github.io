---
title: Automating VSTS from Powershell
subtitle: Getting started with the Teams module
description: Getting started with the Teams module for VSTS in Powershell
author: Harald S. Fianbakken
date: 2017-10-14 12:00
comments: true
avatar: "img/authors/haraldfianbakken.jpg"
header-img: "img/Hackathon.jpg"
layout: post
---

A small blog post in how to get started with automating common operations in VSTS from Powershell and your local machine to avoid GUI as much as possible. 

### Check to see if module is installed
```
    Get-Module -Name Team
```

### Install the module 
```  
   Install-Module -Name Team
```
Note - you can scope this to installation if needed


### Create a PAT token
You will need your account name and logon credentials. 
Let's assume your account name is "vstscontoso". 
Log on to your <a href="https://vstscontoso.visualstudio.com/_details/security/tokens">VSTS account</a> in your browser and create a PAT token.  Select the privileges you like this token to have (and remember if you don't select all of them, certain operations won't work when using this token from Powershell). Make sure to copy this token when created on your clipboard, as you cannot display it again and you'll need it in next step.. 

### Add your account for operations in Powershell
Use your vstscontoso account name and the PAT token generated in the previous step and do the following from Powershell
```
    Add-TeamAccount -Account 'vstscontoso' -PersonalAccessToken 'TOKEN_FROM_PREVIOUS_STEP'
```
You're good to go to start automating! 

### Automate the process
To avoid doing this on every run / console, you would most likely want to add this to your profile. You could consider protecting your key somewhere safe as well (but for simplicity, showing you how in the profile)
```
    "
Add-TeamAccount -Account 'vstscontoso' -PersonalAccessToken 'TOKEN_FROM_PREVIOUS_STEP'" | Add-Content $Profile.CurrentUserAllHosts -Force
```

### Get team projects
```
    Get-Project
```

### Create new project
To create a new team project (with defaults - e.g. GIT and agile template)
```
p = Add-project -Name 'Beerly-Serverless' -Description 'Serverless hackathon'
``` 

### Checkout project
```
    $repo = Get-GitRepository -ProjectName $p.Name
    git clone $repo.RemoteUrl
```

Of course, you could also to all of this in one line if you're a "one liner".


