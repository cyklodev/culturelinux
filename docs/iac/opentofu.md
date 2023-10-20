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
        default =  "https://proxmox.fqdn:8006/api2/json"
    }
    variable  "pm_api_token_id" {
        default =  "terraform@pve!terraform"
    }
    variable  "pm_api_token_secret" {
        default =  "XXXXXXXXXXXXXXXXXXXXXXXX"
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