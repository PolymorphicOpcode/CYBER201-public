# Mystery Box Setup
## REMEMBER TO UPDATE INSERT_PASSWORD_HERE

```powershell

$boxes = @("metasploitable3-ub14 Template", "mrRobot Template","sickos Template","Template ICA1", "win2008vuln")

$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "Mystery." + $y + "2"
    $x = Get-Random -Minimum 0 -Maximum 4

    $create = new-VM -Template (Get-Template -Location 'Godfrey' -Name $boxes[$x]) -Name $VM_name -VMHost '10.11.175.103' -Datastore 'UCS ESXi v101 - SMIF700' -Location "Mystery .2" -ResourcePool "CIT-Instructors" -DiskStorageFormat Thin -confirm:$false

    Wait-Task -Task $create
    write-host "Creation of VM $VM_name completed"

    switch ($boxes[$x]) {
        "metasploitable3-ub14 Template" {
            Invoke-VMScript -vm (get-vm -Name $VM_name) -ScriptText ([string]::Format("echo 'address 10.0.2.{0}2/24' >> /etc/network/interfaces && echo 'root:Mystery.{0}2' | sudo chpasswd", $y)) -GuestUser 'vagrant' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
        }
        "mrRobot Template" {
            Invoke-VMScript -vm (get-vm -Name $VM_name) -ScriptText ([string]::Format("echo 'address 10.0.2.{0}2/24' >> /etc/network/interfaces && echo 'root:Mystery.{0}2' | sudo chpasswd", $y)) -GuestUser 'vagrant' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
        }
        "sickos Template" {
            Invoke-VMScript -vm (get-vm -Name $VM_name) -ScriptText ([string]::Format("echo 'address 10.0.2.{0}2/24' >> /etc/network/interfaces && echo 'root:Mystery.{0}2' | sudo chpasswd", $y)) -GuestUser 'vagrant' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
        }
        "Template ICA1" {
            Invoke-VMScript -vm (get-vm -Name $VM_name) -ScriptText ([string]::Format("echo 'address 10.0.2.{0}2/24' >> /etc/network/interfaces && echo 'root:Mystery.{0}2' | sudo chpasswd", $y)) -GuestUser 'vagrant' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Bash
        }
        "win2008vuln" {
            Invoke-VMScript -vm (get-vm -Name $VM_name) -ScriptText ([string]::Format("net user Administrator Mystery.{0}2;netsh interface ip set address 'Local Area Connection' static 10.0.2.{0}2 255.255.255.0 10.0.2.1 1", $y)) -GuestUser 'vagrant' -GuestPassword 'INSERT_PASSWORD_HERE' -ScriptType Powershell
        }
    }
    write-host "Configuration of $VM_name completed"

}
```