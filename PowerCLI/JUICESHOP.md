## Juice Shop Generation
## MAKE SURE TO REPLACE INSERT_PASSWORD_HERE
```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "JuiceShop." + $y + "3"
    write-host "Creation of VM $VM_name initiated"

    #new-VM -Template (Get-Template -Location 'Godfrey' -Name 'JuiceShop.3 Template') -Name $VM_Name -VMHost '10.11.175.103' -Datastore 'UCS ESXi v101 - SMIF700' -Location "JuiceShop .3" -ResourcePool "CIT-Instructors" -DiskStorageFormat Thin | Set-VM -NumCpu 1 -MemoryGB 2

    #write-host "Power On of the  VM $VM_name initiated"
    #Start-VM -VM $VM_name -confirm:$false

    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'INSERT_PASSWORD_HERE' | sudo -S -u root echo 'kali:JuiceShop.{0}3' | sudo chpasswd", $y)) -GuestUser 'kali' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'JuiceShop.{0}3' | sudo -S -u root sed -i 's/ListenAddress/#ListenAddress/g /etc/ssh/sshd_config '", $y)) -GuestUser 'kali' -GuestPassword 'JuiceShop.{0}3' -ScriptType Bash
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'JuiceShop.{0}3' | sudo -S -u root systemctl start ssh", $y)) -GuestUser 'kali' -GuestPassword 'JuiceShop.{0}3' -ScriptType Bash
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'JuiceShop.{0}3' | sudo -S -u root juice-shop -h", $y)) -GuestUser 'kali' -GuestPassword ([string]::Format("JuiceShop.{0}3", $y)) -ScriptType Bash
}
```

```powershell
$vm_count = "25"
14..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "JuiceShop." + $y + "3"

    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'INSERT_PASSWORD_HERE' | sudo -S -u root sed -i 's/ListenAddress/#ListenAddress/g' /etc/ssh/sshd_config", $y)) -GuestUser 'kali' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'INSERT_PASSWORD_HERE' | sudo -S -u root systemctl start ssh", $y)) -GuestUser 'kali' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'INSERT_PASSWORD_HERE' | sudo -S -u root juice-shop -h", $y)) -GuestUser 'kali' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'INSERT_PASSWORD_HERE' | sudo -S -u root echo 'kali:JuiceShop.{0}3' | sudo chpasswd", $y)) -GuestUser 'kali' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash


    write-host "Configuration of VM $VM_name completed"
}
```
# Set IP addresses across reboot
```powershell
$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "JuiceShop." + $y + "3"

    Invoke-VMScript -vm (get-vm -Name $VM_Name) -ScriptText ([string]::Format("echo 'JuiceShop.{0}3' | sudo -S -u root sed -i 's/10.0.2.22/10.0.2.{0}3/g' /etc/network/interfaces", $y)) -GuestUser 'kali' -GuestPassword ([string]::Format("JuiceShop.{0}3",$y)) -ScriptType Bash

    write-host "IP for $VM_name set"
}
```

# TODO - Fix the passwords

### Attach the virtual guest additions ISO to the VM
```powershell
PS /Users/sam> Get-VM 'JuiceShop .3' | mount-tools

Invoke-VMScript -vm (get-vm 'JuiceShop .3') -ScriptText ([string]::Format("sudo ifconfig eth0 10.0.2.3 && echo 'kali:JuiceShop.3' | sudo chpasswd && sudo juice-shop -h")) -GuestUser 'kali' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
```