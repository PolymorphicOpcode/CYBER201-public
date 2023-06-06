# Scripts for Red Boxes

# Red Machine Generation
## REMEMBER TO UPDATE INSERT_PASSWORD_HERE

```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "Red." + $y + "1"
    write-host "Creation of VM $VM_name initiated"

    new-VM -Template (Get-Template -Location 'Godfrey' -Name 'Red.01') -Name $VM_Name -VMHost '10.11.175.103' -Datastore 'UCS ESXi v101 - SMIF700' -Location "Red .1" -ResourcePool "CIT-Instructors" -DiskStorageFormat Thin | Set-VM -NumCpu 1 -MemoryGB 2 -confirm:$false

    write-host "Power On of the  VM $VM_name initiated"
    Start-VM -VM $VM_name -confirm:$false
}
```

```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "Red." + $y + "1"
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'address 10.0.2.{0}1/24' >> /etc/network/interfaces && echo 'kali:Red.{0}1' | sudo chpasswd", $y)) -GuestUser 'root' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
    write-host "Configuration of $VM_Name complete"
}
```

## Create boxes that work with wpscan updates

```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "RedWP." + $y + "1"
    write-host "Creation of VM $VM_name initiated"

    new-VM -Template (Get-Template -Location 'Godfrey' -Name 'Red.251') -Name $VM_Name -VMHost '10.11.175.103' -Datastore 'UCS ESXi v101 - SMIF700' -Location "Red .1" -ResourcePool "CIT-Instructors" -DiskStorageFormat Thin -confirm:$false
    Start-VM -VM $VM_name -confirm:$false
}

0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "RedWP." + $y + "1"
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'address 10.0.2.{0}1/24' >> /etc/network/interfaces && echo 'kali:Red.{0}1' | sudo chpasswd", $y)) -GuestUser 'root' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
    write-host "Configuration of $VM_Name complete"
}

### Host file edit script

0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "RedWP." + $y + "1"
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("sed -i 's/10.0.2.244/10.0.2.{0}4/g' /etc/hosts", $y)) -GuestUser 'root' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
    write-host "Configuration of $VM_Name complete"
}

### Enable SSHd on all
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "RedWP." + $y + "1"
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("systemctl enable sshd && systemctl start sshd", $y)) -GuestUser 'root' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
    write-host "Configuration of $VM_Name complete"
}
```