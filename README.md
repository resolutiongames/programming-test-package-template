# Fidelity Package Template

## Table of Contents
- [Description](#description)
- [Install](#install)
- [Usage](#usage)
- [Support](#support)
- [Maintainers](#maintainers)
- [Contributing](#contributing)

## Description
This template shall be used when creating a new Fidelity package. The Description section, as well as all other sections in this file shall be filled in with information related to the newly created package.

This template repo intentionally contains no .meta files to avoid conflicts in published packages based on this template.
When using this template, remember to generate .meta-files for all files in your new repo.

## Install
> **_NOTE:_** _Remember to visit <http://gatekeeper.resgam.com> before opening the Package Manager._
1. Open the Unity Package Manager (`Window` -> `Package Manager`) inside the Unity Editor
2. Change the `Packages` tab to `My Registries`, find the package and install it. Dependencies will be installed automatically

## Usage
```csharp
using Fidelity.PackageTemplate;
```
Call the static `World` function in the static `Hello` class like this.
```csharp
Hello.World();
```

## Support
[#fidelity on Resolution Games Slack](https://resolutiongames.slack.com/archives/C02DTQCHY4W)

## Maintainers
[@resolutiongames/fidelity-developers](https://github.com/orgs/resolutiongames/teams/fidelity-developers)

## Contributing
You are always welcome to suggest contributions. Follow [this guide](https://www.notion.so/resolutiongames/Creating-or-Updating-Fidelity-Packages-65194b79fc17452ab8a1d472021d2fc8#5fe877ca59d64191876d52c9ac0b5c25) on Notion.
