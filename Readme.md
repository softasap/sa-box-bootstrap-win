
[![Build Status](https://travis-ci.org/softasap/sa-box-bootstrap-win.svg?branch=master)](https://travis-ci.org/softasap/sa-box-bootstrap-win)


Example of usage:

Simple

```YAML

  vars:
    - box_choco_core_packages:
        - chocolatey-core.extension
        - far
        - git
        - conemu
        - winrar
        - 7zip.install


  roles:
     - {
         role: "sa-box-bootstrap-win",
         timezone: "Central European Standard Time",

         option_install_choco: true,
         choco_core_packages: "{{box_choco_core_packages}}",

         option_install_roaming_profile: true,
         powershell_roaming_profile: "https://github.com/Voronenko/winfiles.git"         

       }


```

Advanced

```YAML

  vars:
    - root_dir: ..
    - timezone: "Central European Standard Time"

    - box_windows_iis_features:
      -  "IIS-BasicAuthentication"
      -  "IIS-DefaultDocument"
      -  "IIS-DirectoryBrowsing"
      -  "IIS-HttpCompressionDynamic"
      -  "IIS-HttpCompressionStatic"
      -  "IIS-HttpErrors"
      -  "IIS-HttpLogging"
      -  "IIS-ISAPIExtensions"
      -  "IIS-ISAPIFilter"
      -  "IIS-ManagementConsole"
      -  "IIS-RequestFiltering"
      -  "IIS-StaticContent"
      -  "IIS-WebSockets"
      -  "IIS-WindowsAuthentication"

    - box_windows_aspnet_features:
      - "NetFx3"
      - "NetFx4-AdvSrvs"
      - "NetFx4Extended-ASPNET45"
      - "IIS-NetFxExtensibility"
      - "IIS-NetFxExtensibility45"
      - "IIS-ASPNET"
      - "IIS-ASPNET45"

    - box_choco_core_packages:
        - chocolatey-core.extension
        - far
        - git
        - conemu
        - winrar
        - 7zip.install



  pre_tasks:
    - debug: msg="Pre tasks section"

  roles:
     - {
         role: "sa-box-bootstrap-win",
         timezone: "Europe/Kiev",

         option_install_choco: true,
         option_install_iis_features: true,
         option_install_aspnet_features: true,

         windows_iis_features: "{{box_windows_iis_features}}",
         windows_aspnet_features: "{{box_windows_aspnet_features}}",
         choco_core_packages: "{{box_choco_core_packages}}",

         option_install_roaming_profile: true,
         powershell_roaming_profile: "https://github.com/Voronenko/winfiles.git"         
         
       }

```

## Windows System Prep

Unless you haven't proceeded yet, make sure you prepared system according steps at  http://docs.ansible.com/ansible/latest/intro_windows.html#windows-system-prep

In order for Ansible to manage your windows machines, you will have to enable and configure PowerShell remoting.
To automate the setup of WinRM, you can run the examples/scripts/ConfigureRemotingForAnsible.ps1 script on the remote machine in a PowerShell console as an administrator.

The example script accepts a few arguments which Admins may choose to use to modify the default setup slightly, which might be appropriate in some cases.

Pass the `-CertValidityDays` option to customize the expiration date of the generated certificate:
```CMD
powershell.exe -File ConfigureRemotingForAnsible.ps1 -CertValidityDays 100
```

Pass the `-EnableCredSSP` switch to enable CredSSP as an authentication option:

```CMD
powershell.exe -File ConfigureRemotingForAnsible.ps1 -EnableCredSSP
```

Pass the `-ForceNewSSLCert` switch to force a new SSL certificate to be attached to an already existing winrm listener. (Avoids SSL winrm errors on syspreped Windows images after the CN changes):

```CMD
powershell.exe -File ConfigureRemotingForAnsible.ps1 -ForceNewSSLCert
```

Pass the `-SkipNetworkProfileCheck` switch to configure winrm to listen on PUBLIC zone interfaces. (Without this option, the script will fail if any network interface on device is in PUBLIC zone):

```CMD
powershell.exe -File ConfigureRemotingForAnsible.ps1 -SkipNetworkProfileCheck
```
To troubleshoot the ConfigureRemotingForAnsible.ps1 writes every change it makes to the Windows EventLog (useful when run unattendedly). Additionally the -Verbose option can be used to get more information on screen about what it is doing.

## Envelope size

If you are going to install big enough packages, you need to increase MaxEnvelopeSizekb setting

```CMD
winrm set winrm/config @{MaxEnvelopeSizekb="8192"}.
```


# Points of interest

You can reuse this playbook to create your own box bootstaping projects, and
reuse the role to configure your environments quicker in secure way with ansible



Usage with ansible galaxy workflow
----------------------------------

If you installed the `sa-box-bootsrap-win` role using the command


`
   ansible-galaxy install softasap.sa-box-bootsrap-win
`

the role will be available in the folder `library/softasap.sa-box-bootsrap-win`
Please adjust the path accordingly.

```YAML

     - {
         role: "softasap.sa-box-bootsrap-win"
       }

```




Copyright and license
---------------------

Code is dual licensed under the [BSD 3 clause] (https://opensource.org/licenses/BSD-3-Clause) and the [MIT License] (http://opensource.org/licenses/MIT). Choose the one that suits you best.

Reach us:

Subscribe for roles updates at [FB] (https://www.facebook.com/SoftAsap/)

Join gitter discussion channel at [Gitter](https://gitter.im/softasap)

Discover other roles at  http://www.softasap.com/roles/registry_generated.html

visit our blog at http://www.softasap.com/blog/archive.html
