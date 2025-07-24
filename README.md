# dev-pc-provisioning

[English](#english) | [日本語](#japanese)

---

<a name="english"></a>
# English

## Overview

This is an Ansible-based provisioning repository for automating development environment setup on macOS, Ubuntu, and Cloud Shell platforms. It uses Ansible roles to install development tools, configure shells, and establish a consistent development environment.

## Prerequisites

- **Homebrew** (macOS): Required for package management. The playbook will check and provide installation instructions if not found.
- **Git SSH key** (optional): Required only if using SSH URLs for dotfiles repository
- **sudo privileges**: Required for system-level changes

## Set up

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

```bash
cd ${Project directory}
ansible-galaxy init --init-path=roles cloudshell
```

## Optional: Installing Go

If you need Go, please install it using one of the following methods:

### macOS
```bash
brew install go
```

### Ubuntu
```bash
# Using Snap (recommended)
sudo snap install go --classic

# Or using the official installer
wget https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.21.5.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
```

### Version Management Tools (Recommended)
For serious Go development, we recommend using version management tools:
- [g](https://github.com/stefanmaric/g) - Simple Go version management
- [goenv](https://github.com/syndbg/goenv) - rbenv-style version management

---

<a name="japanese"></a>
# 日本語

## 概要

これは、macOS、Ubuntu、Cloud Shellプラットフォーム向けの開発環境セットアップを自動化するAnsibleベースのプロビジョニングリポジトリです。Ansibleロールを使用して開発ツールのインストール、シェルの設定、一貫した開発環境の構築を行います。

## 前提条件

- **Homebrew** (macOS): パッケージ管理に必要。未インストールの場合はplaybookがチェックして手順を表示します。
- **Git SSH鍵** (オプション): dotfilesリポジトリにSSH URLを使用する場合のみ必要
- **sudo権限**: システムレベルの変更に必要

## セットアップ

### macOS

1. Homebrewをインストール（未インストールの場合）
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. Ansibleをインストール
   ```bash
   brew install ansible
   ```
3. このリポジトリをクローン
4. `cd dev-pc-provisioning`
5. `ansible-playbook site.yml --ask-become-pass`

### Ubuntu

1. Ansibleをインストール
   ```bash
   sudo apt update && sudo apt install ansible
   ```
2. このリポジトリをクローン
3. `cd dev-pc-provisioning`
4. `ansible-playbook site.yml --ask-become-pass`

### Cloud Shell

1. このリポジトリをクローン
2. `cd dev-pc-provisioning`
3. `ansible-playbook cloudshell.yml --ask-become-pass`

## 初回実行時

初回実行時には以下の入力を求められます：
- **Gitメールアドレス**: Gitコミットで使用。以降の実行ではキャッシュされます。
- **sudoパスワード**: システムレベルの変更に必要（`--ask-become-pass`使用時）

## カスタマイズ

このプロビジョニングを自分用にカスタマイズする場合：

1. `roles/common/defaults/main.yml`を編集：
   ```yaml
   git_user_name: "Your Name"
   github_username: "yourusername"
   ```

2. Git操作でSSHよりHTTPSを好む場合は、playbookまたはvarsでdotfileリポジトリURLを上書きしてください。

## ロールの追加

```bash
cd ${プロジェクトディレクトリ}
ansible-galaxy init --init-path=roles cloudshell
```

## オプション: Go言語のインストール

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