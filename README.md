# Package Template

## Table of Contents
- [Description](#description)
- [Install](#install)
- [Usage](#usage)
- [Authors](#authors)
## Description
This template shall be used when creating a new package. The Description section, as well as all other sections in this file shall be filled in with information related to the newly created package.

This template repo intentionally contains no .meta files to avoid conflicts in published packages based on this template.
When using this template, remember to generate .meta-files for all files in your new repo.

## Install
1. Open the Unity Package Manager (`Window` -> `Package Manager`) inside the Unity Editor
2. Choose `Add a Package` and select `Add Package from git URL...`
```csharp
using MyDomain.PackageTemplate;
```
Call the static `World` function in the static `Hello` class like this.
```csharp
Hello.World();
```

## Authors
Name of the Authors here
