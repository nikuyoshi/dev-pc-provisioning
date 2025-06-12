# dev-pc-provisioning

## Set up

### macOS

1. Install Homebrew
   https://brew.sh/
2. `brew install ansible`
3. Clone this repository
4. `cd dev-pc-provisioning`
5. `ansible-playbook site.yml --ask-become-pass`

### Ubuntu

1. `sudo apt install ansible`
2. Clone this repository
3. `cd dev-pc-provisioning`
4. `ansible-playbook site.yml --ask-become-pass`

## Add role

```
cd ${Project directory}
ansible-galaxy init --init-path=roles cloudshell
```