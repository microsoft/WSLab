# MSLab

## tl;dr

To start using MSLab just download the latest version of the scripts from the [Releases](https://github.com/microsoft/MSLab/releases) section of this repository or follow [Dell GEOS hands-on-labs](https://www.geos.to/azslabs).


💡 Shortcut to the latest version is https://aka.ms/mslab/download

<p align="center">
<a href="https://aka.ms/mslab/download"><img src="https://img.shields.io/static/v1?label=&message=Download+MSLab&color=blue&style=for-the-badge" title="Download MSLab scripts" alt="Download MSLab"></a>
</p>

## Introduction

<!-- TOC -->

- [MSLab](#mslab)
    - [tl;dr](#tldr)
    - [Introduction](#introduction)
    - [Requirements](#requirements)
    - [Scripts](#scripts)
    - [Data Collection](#data-collection)
        - [How to get the Scripts](#how-to-get-the-scripts)
    - [Scenarios](#scenarios)
    - [Use cases](#use-cases)
        - [Prototyping](#prototyping)
        - [Hands on Labs](#hands-on-labs)
        - [Issue reproduction](#issue-reproduction)
        - [Sessions](#sessions)
    - [Run in PowerShell 7](#run-in-powershell-7)
    - [Execution Policy](#execution-policy)
    - [Linux preview](#linux-preview)

<!-- /TOC -->

MSLab is a GitHub project that aims to provide virtual environments in Hyper-V, that can be built in a consistent way. It comes at no additional cost, it's free and available under the [MIT License](License).

Unlike other solutions, MSLab focuses on simplicity (all actions can be done without typing complex scripts) and low profile (all disks are differencing, minimum requirements are 8GB RAM and 40GB free space). There is no special hardware requirement. MSLab can run on almost any machine that has SSD and decent amount of memory.

![](Docs/media/Hyper-V_Manager01.png)

## Requirements

* Windows 10 Pro/Enterprise (as Hyper-V is required) or Windows Server 2016/2019
* 8GB RAM
* CPU with Virtualization support
* SSD
* 40GB free space

## Scripts

The main part of MSLab are the [Scripts](https://aka.ms/mslabzip) that will help you preparing lab files ([Hydration Phase](Docs/mslab-hydration.md)). This phase is the most time consuming (1-2hours), but needs to be done only once. It will create virtual hard disks from of provided ISO and will create Domain Controller. [MSLab Deployment](Docs/mslab-deployment.md) takes only few minutes as it will just import Domain Controller and will add other Virtual Machines as specified in LabConfig.ps1

![](Docs/media/Explorer01.png)

## Data Collection

The software may collect information about you and your use of the software and send it to Microsoft. Microsoft may use this information to provide services and improve our products and services. You may turn off the telemetry as [described in the repository](http://aka.ms/mslab/telemetry). There are also some features in the software that may enable you and Microsoft to collect data from users of your applications. If you use these features, you must comply with applicable law, including providing appropriate notices to users of your applications together with a copy of Microsoft's privacy statement. Our privacy statement is located at https://go.microsoft.com/fwlink/?LinkID=824704. You can learn more about data collection and use in the help documentation and our privacy statement. Your use of the software operates as your consent to these practices.

### How to get the Scripts

In the past, this ZIP file was stored in the git repository, and recently we switched to using a native Releases feature of Github, where all releases are available at https://github.com/microsoft/MSLab/releases. Also, we have a static direct link to the latest MSLab release on http://aka.ms/mslab/download which is updated automatically when we create a new version.

This built ZIP file is more optimized, e. g. the file 0_Shared.ps1 is in-lined to the rest of the scripts to keep the number of MSLab files as low as possible. Compared to the git repository where I tend to split those scripts to multiple independent files for a better supportability on our side.

## Scenarios

Over the time, we have developed multiple [scenarios](Scenarios/) simulating Azure Stack HCI and even deep dives into other technologies such as [Windows Admin Center](Scenarios/Windows%20Admin%20Center%20and%20Enterprise%20CA), [Certification Authority](Scenarios/Certification%20Authority) or [Just Enough Administration](Scenarios/BitLocker%20with%20JEA). Scenarios can be reused for real environments. For example [S2D Hyperconverged](Scenarios/S2D%20Hyperconverged) can be used to deploy real Azure Stack HCI clusters.

## Use cases

### Prototyping

MSLab is ideal for prototyping. It will quickly spin Windows Server/Client environments that are connected to Internet and ready to be played with. If something goes wrong, there is nothing easier than just run Cleanup and then Deploy again

### Hands on Labs

MSLab virtual machines are defined in LabConfig.ps1 as simple hash table. This way you can share your configurations and create consistent, complex environments. Labs are easy to distribute. Once you are done with Hydration Phase, you can just copy result folder to multiple computers and deliver entire classes.

### Issue reproduction

Many times happened, that there was an issue that was hard to reproduce. And even if you could reproduce it, another person did not have the same environment, so even with the same steps, the issue might not occur again. MSLab changes this as all MSLab environments with the same LabConfig are the same. So only thing you need to share to other person to reproduce issue are steps and LabConfig.ps1.

### Sessions

For more session slides navigate to [Slides](https://1drv.ms/u/s!AjTsLJdE37DwtrsnIehxKx7N7XgoBg?e=r1sszn) OneDrive

[![MSLab in MVPDays](/Docs/media/Deploying_AzSHCI_with_MDT.png)](https://youtu.be/Vipbhkv9wyM)

[![MSLab with Carsten](/Docs/media/Create_great_demo_environments.png)](https://youtu.be/f3EH2NOM2Eg)

[![MSLab in MVPDays](/Docs/media/Monitoring_Azure_Stack_HCI_with_Grafana_thumb.png)](https://youtu.be/0K53z4LMT4U)

[![MSLab in MVPDays](/Docs/media/S2DSimulations_presentation_thumb.png)](https://youtu.be/u7d6Go8weBc)

[![MSLab in CDCGermany](/Docs/media/MSLab_Datacenter_Simulation_presentation_thumb.png)](https://youtu.be/5IX9OLEk50Q)

## Run in PowerShell 7
>[!Note] 
>These steps are obsolete. PS 7 will integrate itself into the Windows 10 / 11 context menu, aswell as the modern Windows 11 context menu.
>
>"Run with PowerShell" will follow your defaults settings in Microsoft Terminal. It's optional, but recommended, to setup and using Microsoft Terminal as your default shell environment, for the best PowerShell 7 UX.


MSLab scripts work also in PowerShell 7, if you want to test it just install latest version of PowerShell 7.

If you also would like to have context menu integration like this:
![](Docs/media/Explorer02pwsh.png) 

You can use this script to register PowerShell Core integration in Explorer.

```powershell
# Set context menu option
$pwshPath = "c:\Program Files\PowerShell\7-preview\pwsh.exe"

if(-not (Get-PSDrive -PSProvider Registry | Where-Object Root -EQ "HKEY_CLASSES_ROOT")) {
    New-PSDrive -PSProvider Registry -Root "HKEY_CLASSES_ROOT" -Name "HKCR"
}

New-Item -Path "HKCR:\Microsoft.PowerShellScript.1\Shell" -Name "1"
New-ItemProperty -Path "HKCR:\Microsoft.PowerShellScript.1\Shell\1" -PropertyType String -Name "MUIVerb" -Value "Run with PowerShell &Core"
New-ItemProperty -Path "HKCR:\Microsoft.PowerShellScript.1\Shell\1" -PropertyType String -Name "Icon" -Value $pwshPath

New-Item -Path "HKCR:\Microsoft.PowerShellScript.1\Shell\1" -Name "Command"
Set-ItemProperty -Path "HKCR:\Microsoft.PowerShellScript.1\Shell\1\Command" -Name "(Default)" -Value ('"{0}" "-Command" "if((Get-ExecutionPolicy ) -ne ''AllSigned'') {{ Set-ExecutionPolicy -Scope Process Bypass }}; & ''%1''"' -f $pwshPath)

```

## Execution Policy
If your environment enforces running signed PowerShell scripts, scripts in release ZIP archive (starting from September 2022) are now signed with a code signing certificate. Although the code signing certificate is trusted you might see this warning when running the MSLab scripts:
![](Docs/media/mslab-sign-allow.png) 
This is by design behavior of PowerShell runtime, as certificates for scripts are stored in separate certificate store (`Cert:\CurrentUser\TrustedPublisher\`) and explicit decision is required for each certificate.

Also please keep in mind that any change to `LabConfig.ps1` file would then require to sign that file again as any change in LabConfig would invalidate initial signature.

To sign `LabConfig.ps1` file you can use this snippet that would select first available Code Signing certificate on your computer:
```powershell
# Get a Code signing certificate from store
$certificate = Get-ChildItem -Path Cert:\CurrentUser\My\ -CodeSigningCert | Select-Object -First 1

# Add signature to a LabConfig file
Set-AuthenticodeSignature -FilePath "LabConfig.ps1" -Certificate $certificate

```

## Linux (preview)

There is en experimental support for building Linux parent images in MSLab. For building those images MSLab use [Packer](https://www.packer.io/) tool. Supported Packer templates are hosted in separate GitHub repository https://github.com/microsoft/MSLab-templates.

To build a Linux parent disk `Linux = $true` need to be added to the `LabConfig.ps1` before running any MSLab scripts. When  `1_Prereq.ps1` is ran MSLab would download Packer if not present on the machine yet, generates SSH key pair unique per a MSLab instance folder. This SSH key will be hardcoded in every parent disk built by that instance. 

After prerequisites stage additional PowerShell script `CreateLinuxParentDisk.ps1` will be `ParentDisks` folder. You can use that script to build a Linux parent disk in the similar way like the Windows images. 

You can also use your own SSH key that can be shared by multiple MSLab instances by explicitely specifying a path to it using `SshKeyPath` option in `LabConfig.ps1`.

`Deploy.ps1` script is using `hv_socket` to connect to a Linux instances and provision them online (similar to how PowerShell Direct work for Windows virtual machines). By default, Linux virtual machines would also be joined to MSLab Active Directory via [sssd](https://sssd.io/) tool.

All the supported Linux distributions and their Packer templates are in the [Microsoft/MSLab-templates](https://github.com/microsoft/MSLab-templates) repository. Should you run to any problem with specific distribution, please open an issue directly in that repository.
