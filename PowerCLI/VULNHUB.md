## REMEMBER TO UPDATE INSERT_PASSWORD_HERE 
```powershell
Invoke-VMScript -vm (get-vm 'DC-2 .4') -ScriptText ([string]::Format("sudo ifconfig eth0 10.0.2.4 && echo 'root:DC.4' | sudo chpasswd")) -GuestUser 'root' -GuestPassword 'DC.4' -ScriptType Bash
```

# VulnHub Machine Generation
# TODO - Automated integration of OVF pulling?
```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "DC2." + $y + "4"
    write-host "Creation of VM $VM_name initiated"

    new-VM -Template (Get-Template -Location 'Godfrey' -Name 'DC2 .4 Template') -Name $VM_Name -VMHost '10.11.175.103' -Datastore 'UCS ESXi v101 - SMIF700' -Location "DC2 .4" -ResourcePool "CIT-Instructors" -DiskStorageFormat Thin | Set-VM -NumCpu 1 -MemoryGB 2 -confirm:$false

    write-host "Power On of the  VM $VM_name initiated"
    Start-VM -VM $VM_name -confirm:$false
}
```

```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "DC2." + $y + "4"
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("ifconfig eth0 10.0.2.{0}4 && echo 'root:DC.{0}4' | chpasswd", $y)) -GuestUser 'root' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
    write-host "Initialization of $VM_name succeeded"
}
```