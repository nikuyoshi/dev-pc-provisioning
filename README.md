# dev-pc-provisioning

1. Install Homebrew
    https://brew.sh/
2. `brew install ansible`
3. `ansible-playbook site.yml`

## Add role

```
cd ${Project directory}
ansible-galaxy init --init-path=roles cloudshell
```