---
title: "Update npm packages"
date: 2019-01-16
tags: ["npm","yarn"]
topics: ["node"]
---

Given the rapid pace of changes, it is a given that we'll need to update our projects' npm packages eventually. There are several different techniques we can use to update a NodeJs project:

#### Built-in Utilities
We can check for outdated packages by running the command line <mark style="background-color: #e8f0fc">npm outdated</mark> in the project's root folder.  Executing another command line <mark style="background-color: #e8f0fc">npm update</mark> will update the packages according to the semantic versioning (semver) restraints

#### Third-Party Tools
<pre>*1. npm-check-updates*</pre>
Using npm-check-updates (ncu) require installing a package:
````
npm install -g npm-check-updates
````
Run command line <mark style="background-color: #e8f0fc">ncu</mark> will show the latest packages versions available.  Executing <mark style="background-color: #e8f0fc">ncu -u</mark> will **only** update the package.json file. The packages are updated to the latest versions, ignoring the semver restraints. Afterward, we'll need to run the additional command line <mark style="background-color: #e8f0fc">npm install</mark> to actually update the npm packages in the node_modules folder.

<pre>*2. npm-check*</pre>
Using npm-check require installing a different package:
````
npm install -g npm-check
````
Running the command line <mark style="background-color: #e8f0fc">npm-check -u</mark>
will let you interactively select which packages to upgrade. It will upgrade the package(s) to the latest version(s), ignoring the semver restraints.

#### Yarn
The command <mark style="background-color: #e8f0fc">yarn outdated</mark> will list the latest versions available. Run <mark style="background-color: #e8f0fc">yarn upgrade-interactive</mark> will yield an interactive screen allowing you to select which packages to upgrade.
