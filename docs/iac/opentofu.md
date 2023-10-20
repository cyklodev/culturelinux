# OpenToFu
## Install
    wget https://github.com/opentofu/opentofu/releases/download/v1.6.0-alpha3/tofu_1.6.0-alpha3_linux_amd64.zip
    unzip tofu*
    cp tofu /usr/local/bin/

## Files 
### provider.tf
    terraform {
        required_providers {
            proxmox =  {
            source = "Telmate/proxmox"
            version = "2.9.11"
            }
        }
    }
    provider "proxmox" {
        pm_api_url = var.pm_api_url
        pm_api_token_id= var.pm_api_token_id
        pm_api_token_secret= var.pm_api_token_secret
        pm_tls_insecure = true
    }

### vars.tf
    variable  "pm_api_url" {
        default =  "https://$PROXMOX_URL:8006/api2/json"
    }
    variable  "pm_api_token_id" {
        default =  "terraform@pve!terraform"
    }
    variable  "pm_api_token_secret" {
        default =  "$TOKEN"
    }
    variable  "target_node" {
        default =  "proxmox2"
    }
    variable "clone" {
        default =  "debian12-temp"
    }
    variable  "ssh_key" {
        default =  "ssh-ed25519 XXXXXXXXXXXXXXXX clinux@mothership"
    }

### main.tf
    resource "proxmox_vm_qemu" "tp_servers" {
    desc = "Deploiement VM Debian 12 sur Proxmox"
    count = 1
    name = "prox2-debian-${count.index + 1}"
    target_node = var.target_node
    clone = var.clone
    os_type = "cloud-init"
    cores = 2
    sockets = 1
    cpu = "host"
    memory = 2048
    scsihw = "virtio-scsi-pci"
    bootdisk = "scsi0"

    disk {
        slot = 0
        size = "40G"
        type = "scsi"
        storage = "local-lvm"
        #iothread = 1
    }

    network {
        model = "virtio"
        bridge = "vmbr0"
    }

    ipconfig0 = "ip=192.168.1.${count.index + 170}/24,gw=192.168.1.1"
    nameserver = "192.168.1.20"
    searchdomain = "local.cyklodev.com"

    sshkeys = var.ssh_key
    }