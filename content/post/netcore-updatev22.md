---
title: "Steps to update .NET Core app from v2.1 to v2.2"
date: 2018-12-10
tags: ["dotnet","VS Code"]
topics: ["net core"]
---

Here are the steps I took to update a recent project:
<ol>
  <li>
  Install the .NET Core 2.2 SDK from https://dotnet.microsoft.com/download/dotnet-core/2.2. The SDK download will also install the runtime.
  </li>
  <li>
  Update the TargetFramework attribute in the project's .csproj file:
  ```
  <TargetFramework>netcoreapp2.2</TargetFramework>
  ```      
  </li>
  <li>
  Run the command line ```dotnet list package --outdated``` to find what packages might need to be updated.  With certain packages, you're required to update them or the project won't compiled (ie dotnet build).
  In my case, I had to update two packages from version 2.1.0 to 2.2.0:
  ```
  <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="2.2.0" />
  <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="2.2.0" />
  ```
  </li>
  <li>
  Run the command line ```dotnet build```.  This command will implicitly run the command ```dotnet restore```, which does the actual work of updating the packages, and then it will compile the application.
  </li>
  <li>
  Update the correct path to the project's dll in the .vscode\launch.json file.  Since I used Visual Studio Code as my editor, I updated the path to make sure debugging continue to work correctly:  
  ```
  "program": "${workspaceFolder}\\bin\\Debug\\netcoreapp2.2\\ExpenseMgmt.dll"
  ```
  </li>
</ol>    