---
title: "Automate IIS Sites with PowerShell"
date: 2019-05-06
tags: ["iis"]
topics: ["powershell"]
---

Converting an IIS virtual directory to web application requires a number of steps. If tens or hundreds of sites are involved and done manually, this can be a tedious, error prone and time-consuming endeavor. Luckily, PowerShell can automate those tasks with relative ease :-).

```
Import-Module WebAdministration

foreach ($appName in Get-Content .\SitesList.txt) {
    Write-Host $appName -fore green;

	# $appName = "MyApp"
	$appPoolName = "AppPool_$appName"
	$sitePath = "example.com"
	$folderPath = "C:\inetpub\example.com\$appName"
	$iisPath = "IIS:\Sites\example.com\$appName"

	# 1. Create App Pool
	New-Item -Path IIS:\AppPools\$appPoolName
	# 2. Convert to Application
	New-WebApplication -Name $appName -Site $sitePath -PhysicalPath $folderPath -ApplicationPool $appPoolName
	# 3. To disable anonymous authentication
	Set-WebConfigurationProperty -filter "/system.WebServer/security/authentication/anonymousAuthentication" -Name Enabled -Value False -PSPath $iisPath
	# 4. To enable windows authentication
	Set-WebConfigurationProperty -filter "/system.WebServer/security/authentication/windowsAuthentication" -Name Enabled -Value True -PSPath $iisPath

	# 5. Set Folder Permissions: Add App Pool Identity User
	$appPoolSid = (Get-ItemProperty IIS:\AppPools\$appPoolName).applicationPoolSid
	Write-Output "App Pool User $appPoolSid"
	
	$identifier = New-Object System.Security.Principal.SecurityIdentifier $appPoolSid
	$user = $identifier.Translate([System.Security.Principal.NTAccount])

	Write-Output "Translated User $user"

	$acl = Get-Acl $folderPath
	# $acl.SetAccessRuleProtection($True, $False)
	$rule = New-Object System.Security.AccessControl.FileSystemAccessRule($user,"Modify, Synchronize", "ContainerInherit, ObjectInherit", "None", "Allow")
	# $rule = New-Object System.Security.AccessControl.FileSystemAccessRule("IIS APPPOOL\$appPoolName","Modify, Synchronize", "ContainerInherit, ObjectInherit", "None", "Allow")
	$acl.AddAccessRule($rule)

	$acl | set-acl -path $folderPath

	# Get-Acl $folderPath  | Format-List	
} 
```

The file **SitesList.txt** contains the list of directory names to be converted to web applications.