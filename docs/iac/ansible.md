# ansible
## install
    sudo dnf install pipx
    pipx install --include-deps ansible
    pipx inject --include-apps ansible argcomplete
## Upgrade
    pipx upgrade --include-injected ansible
## Test
    python3 -m pip -V
    ansible --version
    ansible-community --version
## Config
    ansible-config init --disabled -t all > ansible.cfg

# inventory
## setup
    # single host
    bpg-debian12 ansible_host=192.168.1.170 ansible_user=debian
## test
    ansible bpg-debian12 -m ping -i inventory
    ansible all -m ping -i inventory
## envvar
    export ANSIBLE_INVENTORY=inventory
    
# playbook
## create 
```
---
- name: Test user
  hosts: bpg-debian12
  remote_user: debian
  become: yes

  tasks:
  - name: test user
    shell: id
    register: results
  - debug:
      var: results.stdout
```
## Execute
    ansible-playbook plb_get_user.yml -i inventory
    export ANSIBLE_INVENTORY=inventory
    ansible-playbook plb_get_user.yml