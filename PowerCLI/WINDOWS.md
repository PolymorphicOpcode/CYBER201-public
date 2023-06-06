## Windows Server 2008 Metasploitable Generation

```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "Windows." + $y + "2"
    write-host "Creation of VM $VM_name initiated"
    new-VM -Template (Get-Template -Location 'Windows .2' -Name 'win2008vuln') -Name $VM_Name -VMHost '10.11.175.103' -Datastore 'UCS ESXi v101 - SMIF700' -Location "Windows .2" -ResourcePool "CIT-Instructors" -DiskStorageFormat Thin -confirm:$false
    Start-VM -VM $VM_name -confirm:$false
    write-host "Startup of $VM_name completed"
}

1..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "Win2k8." + $y + "2"
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("net user Administrator Windows.{0}2;netsh interface ip set address 'Local Area Connection' static 10.0.2.{0}2 255.255.255.0 10.0.2.1 1;Rename-Computer -NewName Windows{0}2 -Restart", $y) -replace '02','2') -GuestUser 'vagrant' -GuestPassword 'vagrant' -ScriptType Powershell
    write-host "Configuration of $VM_Name complete"
}
```