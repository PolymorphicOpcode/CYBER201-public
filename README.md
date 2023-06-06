# CYBER201

## The following are meant to be run within a Powershell environment with PowerCLI installed

### Documentation for the PowerCLI commands can be found [here](https://developer.vmware.com/docs/powercli/latest/products/vmwarevsphereandvsan/)
```powershell
Install-Module -Name VMware.PowerCLI
```

## First time connection
```powershell
Connect-VIServer "INSERT-SERVER"
```

## Continued connection
```powershell
Connect-VIServer -Menu

Get-Folder -Name 'CYBER201'

Get-Template
```

## Creating a VM from a template
```powershell
new-VM -Template "JuiceShop.x" -Name 'SamTest' -VMHost '10.11.175.103' -Datastore 'UCS ESXi v101 - SMIF700' -Location "CYBER201" -ResourcePool "CIT-Instructors" -DiskStorageFormat Thin | Set-VM -NumCpu 2 -MemoryGB 4
```

# 10.0.2.x/24
### .1 Red
### .2 Victim
### .3 JS
### .4 VulnHub

## Create 25 Virtual Machines from the Template and configure IP addresses accordingly

## Set Machine hostname
```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "JuiceShop." + $y + "3"

    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'JuiceShop.{0}3' | sudo -S -u root hostnamectl set-hostname juiceshop", $y)) -GuestUser 'kali' -GuestPassword ([string]::Format("JuiceShop.{0}3",$y)) -ScriptType Bash

    write-host "Hostname for $VM_name set"
}
```
## Docs overview for Terraform AWS 
(https://registry.terraform.io/providers/hashicorp/aws/latest/docs#AWS_SHARED_CREDENTIALS_FILE)[https://registry.terraform.io/providers/hashicorp/aws/latest/docs#AWS_SHARED_CREDENTIALS_FILE]

## VPC-Specific resources
(https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)[https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc]

