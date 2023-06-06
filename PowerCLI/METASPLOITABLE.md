# Windows Vuln Box Generation
## REMEMBER TO UPDATE INSERT_PASSWORD_HERE

# Get-VM 'XP Test' | mount-tools # How to mount vmware-tools via powercli
```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "Win2k8." + $y + "2"
    write-host "Creation of VM $VM_name initiated"
    new-VM -Template (Get-Template -Location 'Win2k8 .2' -Name 'metasploitable3-win2k8') -Name $VM_Name -VMHost '10.11.175.103' -Datastore 'UCS ESXi v101 - SMIF700' -Location "Win2k8 .2" -ResourcePool "CIT-Instructors" -DiskStorageFormat Thin -confirm:$false
    write-host "Power On of the  VM $VM_name initiated"
    Start-VM -VM $VM_name -confirm:$false
}
```
    #| 5/4/2023 1:32:13 PM	Start-VM		Permission to perform this operation was denied. Required privileges: 'PropertyCollector-propertyCollector' : 'System.Read'	
```powershell
1..$vm_count | foreach { #only doing from 1, configured .02 manually because the IP configuration wasn't cooperating with .02, may be worth looking into turnicating that leading 0
    $y="{0:D1}" -f + $_
    $VM_name= "Win2k8." + $y + "2"
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("net user Administrator Win2k8.{0}2;netsh interface ip set address 'Local Area Connection' static 10.0.2.{0}2 255.255.255.0 10.0.2.1 1", $y)) -GuestUser 'Administrator' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Powershell
    write-host "Configuration of $VM_Name complete"
}
```
#Make sure to restart VMs after configuration

# Ubuntu Metasploitable JuiceShop Setup
```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "Ubuntu." + $y + "3"
    write-host "Creation of VM $VM_name initiated"
    new-VM -Template (Get-Template -Location 'Godfrey' -Name ubuntuvuln) -Name $VM_Name -VMHost '10.11.175.103' -Datastore 'UCS ESXi v101 - SMIF700' -Location "Metasploitable-JuiceShop .5" -ResourcePool "CIT-Instructors" -DiskStorageFormat Thin -confirm:$false
}
```

```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "Ubuntu." + $y + "3"
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'ubuntu:Ubuntu.{0}3' | sudo chpasswd && juice-shop -h", $y)) -GuestUser 'root' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
}
```