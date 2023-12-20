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
## create test
```
---
- name : test user
  hosts: bpg-debian12
  remote_user: debian
  become: true

  tasks:
  - name: get id
    shell: id
    register: results
  - name: Add the user 'clinux'
    ansible.builtin.user:
      name: clinux
      comment: created by ansible
  - name: break
    shell: exit 1
    ignore_errors: true
  - name: Remove the user 'clinux'
    ansible.builtin.user:
      name: clinux
      state: absent
      remove: yes

  - name: Install some packages
    package:
      name:
        - htop
        - curl
        - nmon
        - net-tools
        - nginx
      state: latest

  - name: Make sure a service unit is running
    ansible.builtin.systemd_service:
      state: started
      enabled: true
      name: nginx

  - debug:
      var: results.stdout 
```
## create post tofu root
```
---
- name: "tofu : setup root key"
  ansible.builtin.copy:
    remote_src: true
    src: /home/debian/.ssh/authorized_keys
    dest: /root/.ssh/authorized_keys
    owner: root
    group: root
    mode: '0600'

- name: "tofu : allow root sshkey"
  lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '(.*)PermitRootLogin(.*)$'
    line: 'PermitRootLogin without-password'
    owner: root

- name: "tofu : reload ssh"
  service:
    name: ssh
    state: restarted
```

## Execute
    ansible-playbook plb_get_user.yml -i inventory
    export ANSIBLE_INVENTORY=inventory
    ansible-playbook plb_get_user.yml


# Roles 
## Create 
    ansible-galaxy init /etc/ansible/roles/debian_tofu_post --offline