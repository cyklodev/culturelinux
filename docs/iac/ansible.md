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
    