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