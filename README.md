# dev-pc-provisioning

## Set up

### Prerequisites

- **Homebrew** (macOS): Required for package management. The playbook will check and provide installation instructions if not found.
- **Git SSH key** (optional): Required only if using SSH URLs for dotfiles repository
- **sudo privileges**: Required for system-level changes

### macOS

1. Install Homebrew (if not already installed)
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. Install Ansible
   ```bash
   brew install ansible
   ```
3. Clone this repository
4. `cd dev-pc-provisioning`
5. `ansible-playbook site.yml --ask-become-pass`

### Ubuntu

1. Install Ansible
   ```bash
   sudo apt update && sudo apt install ansible
   ```
2. Clone this repository
3. `cd dev-pc-provisioning`
4. `ansible-playbook site.yml --ask-become-pass`

### Cloud Shell

1. Clone this repository
2. `cd dev-pc-provisioning`
3. `ansible-playbook cloudshell.yml --ask-become-pass`

## First Run

On first run, you will be prompted for:
- **Git email address**: Used for Git commits. This will be cached for future runs.
- **sudo password**: Required for system-level changes (when using `--ask-become-pass`)

## Customization

To use this provisioning for your own setup:

1. Edit `roles/common/defaults/main.yml`:
   ```yaml
   git_user_name: "Your Name"
   github_username: "yourusername"
   ```

2. If you prefer HTTPS over SSH for Git operations, override the dotfile repository URL in your playbook or vars.

## Add role

```
cd ${Project directory}
ansible-galaxy init --init-path=roles cloudshell
```

## Optional: Go言語のインストール

Go言語が必要な場合は、以下の方法でインストールしてください：

### macOS
```bash
brew install go
```

### Ubuntu
```bash
# Snapを使用（推奨）
sudo snap install go --classic

# または公式インストーラー使用
wget https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.21.5.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
```

### バージョン管理ツール（推奨）
Go開発を本格的に行う場合は、バージョン管理ツールの使用を推奨します：
- [g](https://github.com/stefanmaric/g) - シンプルなGoバージョン管理
- [goenv](https://github.com/syndbg/goenv) - rbenvスタイルのバージョン管理