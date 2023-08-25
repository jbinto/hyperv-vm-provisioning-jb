# hyperv-vm-provisioning-jb

Forked from https://github.com/schtritoff/hyperv-vm-provisioning

## usage

Prereq:
* Windows 10+ computer with Hyper-V Manager installed
* A Hyper-V virtual switch called "Bridge" (optional)
* This repo

```powershell
.\New-HyperVCloudImageVM.ps1 `
    -VirtualSwitchName "Bridge" `
    -VMVersion "11.0" `
    -VMProcessorCount 12 `
    -VMMemoryStartupBytes 32GB `
    -VHDSizeBytes 8GB `
    -VMName "operator.lynx-catfish.ts.net" `
    -ImageVersion "22.04" `
    -VMGeneration 2 `
    -CustomUserDataYamlFile "./ansible.yaml" `
    -VMMachine_StoragePath "T:\VIRTUAL\" `
    -Verbose `
    -ShowSerialConsoleWindow
```

For about 5 minutes, it will set up a VM. A PuTTY window will appear with output from a named pipe. It will appear to halt for a bit, but then it will output more and reboot. At this point, connect to it with ssh (see the ssh keys specified in `ansible.yaml`). 

To find the ip address, you will have to scroll to the point where `ci-info` outputs networking information, e.g

```
[   19.430172] cloud-init[870]: ci-info: ++++++++++++++++++++++++++++++++++++++Net device info+++++++++++++++++++++++++++++++++++++++
[   19.438234] cloud-init[870]: ci-info: +--------+------+-----------------------------+---------------+--------+-------------------+
[   19.443126] cloud-init[870]: ci-info: | Device |  Up  |           Address           |      Mask     | Scope  |     Hw-Address    |
[   19.448438] cloud-init[870]: ci-info: +--------+------+-----------------------------+---------------+--------+-------------------+
[   19.454576] cloud-init[870]: ci-info: |  eth0  | True |        192.168.43.120       | 255.255.255.0 | global | 00:15:5d:2b:de:1c |
```

TODO: configure this to use a static ip address.

Can SSH to it with putty no problem. 

Next tasks:
- bootstrap Tailscale, such that it can listen on ssh
- an Ansible playbook to run docker-compose nginx/gitea/etc.

