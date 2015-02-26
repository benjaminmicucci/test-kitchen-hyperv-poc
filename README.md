## Test Kitchen With Hyper-V Proof of Concept

### Pre-Requisites

- [ChefDK](https://downloads.chef.io/chef-dk/)
- [Vagrant](http://www.vagrantup.com/downloads.html)

### Hyper-V Set Up

Enable Hyper-V:

```powershell
PS > Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All
```

__Note__: Once you enable Hyper-V, you will no longer be able to use VirtualBox.  See [this post](http://www.hanselman.com/blog/SwitchEasilyBetweenVirtualBoxAndHyperVWithABCDEditBootEntryInWindows81.aspx) for a workaround.

Then create the virtual switch:

```powershell
PS > Import-Module Hyper-V
PS > Get-NetAdapter | where { $_.Status -eq "Up" }   # to list adapter names
PS > New-VMSwitch -Name vagrant -NetAdapterName <ADAPTER_NAME> -AllowManagementOS $true
```

Where `ADAPTER_NAME` is the name of the adapter you use to connect to the internet.

### Running Test Kitchen

1. Run PowerShell as Administrator (this is required for interacting with Hyper-V)
2. `cd` to the checked out directory
3. Run `chef exec kitchen test`

An Ubuntu VM should now spin up in Hyper-V and run the stock tests.
