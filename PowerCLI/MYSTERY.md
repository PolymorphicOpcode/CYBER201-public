# Mystery Box Setup

```powershell

$boxes = @("metasploitable3-ub14 Template", "mrRobot Template","sickos Template","Template ICA1", "win2008vuln")

$vm_count = "25"
0..$vm_count | foreach {
    $y="{0:D1}" -f + $_
    $VM_name= "Mystery." + $y + "2"
    $x = Get-Random -Minimum 0 -Maximum 4

    $create = new-VM -Template (Get-Template -Location 'Godfrey' -Name $boxes[$x]) -Name $VM_Name -VMHost '10.11.175.103' -Datastore 'UCS ESXi v101 - SMIF700' -Location "Mystery .2" -ResourcePool "CIT-Instructors" -DiskStorageFormat Thin -confirm:$false

    Wait-Task -Task $create
    write-host "Creation of VM $VM_name completed"
}
```