# IOMMU Support
Credit to [Craft Computing](https://www.youtube.com/@CraftComputing) for this portion of the documentation

## Legacy Systems

```
nano /etc/default/grub
```

<br>For Intel Systems

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```

<br>OR for AMD Systems

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"
```

<br>Save file and close

```
update-grub
```

## EFI Boot Systems

Intel

```
nano /etc/kernel/cmdline

%> intel_iommu=on
```


<br>OR for AMD

```
nano /etc/kernel/cmdline

%> amd_iommu=on
```

<br>Save file and close

```
proxmox-boot-tool refresh
```


## Load VFIO modules at boot-

```
nano /etc/modules
```

```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

Save file and close


## Blacklist graphic drivers (optional)-
```
echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
```
```
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
```

## Apply all changes

```
update-initramfs -u -k all
```

Reboot