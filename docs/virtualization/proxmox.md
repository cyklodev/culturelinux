# Proxmox

## Template image
### pveum - Proxmox VE User Manager
#### Add role
    pveum role add Terraform -privs "VM.Allocate VM.Clone VM.Config.CDROM VM.Config.CPU VM.Config.Cloudinit VM.Config.Disk VM.Config.HWType VM.Config.Memory VM.Config.Network VM.Config.Options VM.Monitor VM.Audit VM.PowerMgmt Datastore.AllocateSpace Datastore.Audit"
#### Add user
    pveum user add terraform@pve -password XXXXXXXX -comment "Terraform account"
#### link role user
    pveum aclmod / -user terraform@pve -role Terraform
#### generate token
    pveum user token add terraform@pve terraform -expire 0 -privsep 0 -comment "Terraform token"
#### test tocken
    curl -X GET 'https://$PROXMOX_URL:8006/api2/json/nodes' -H 'Authorization: PVEAPIToken=terraform@pve!terraform=$TOKEN'
## Build template
### get qcow image
    cd /tmp && wget https://cloud.debian.org/images/cloud/bookworm/20231013-1532/debian-12-genericcloud-amd64-20231013-1532.qcow2
### build template vm
    qm create 1000 --memory 1024 --core 1 --name debian12-temp --net0 virtio,bridge=vmbr0 --description "Debian 12 cloud template"
    qm importdisk 1000 /tmp/debian-12-genericcloud-amd64-20231013-1532.qcow2 local-lvm
    qm set 1000 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-1000-disk-0
    qm set 1000 --boot c --bootdisk scsi0
    qm set 1000 --ide2 local-lvm:cloudinit
    qm set 1000 --serial0 socket --vga serial0
    rm -f /tmp/debian-12-genericcloud-amd64-20231013-1532.qcow2

