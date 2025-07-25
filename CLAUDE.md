# CLAUDE.md

このファイルは、このリポジトリでコード作業を行う際のClaude Code (claude.ai/code) への指針を提供します。

## リポジトリ概要

これは、macOS、Ubuntu、Cloud Shellプラットフォーム向けの開発環境セットアップを自動化するAnsibleベースのプロビジョニングリポジトリです。Ansibleロールを使用して開発ツールのインストール、シェルの設定、一貫した開発環境の構築を行います。

## 重要な注意事項

このリポジトリは個人用（Hiroki Uchida）の開発環境セットアップ用です。個人設定は`roles/common/defaults/main.yml`で変数化されており、簡単にカスタマイズ可能です。

### 既知の問題
- **Cloud Shell dotfiles**: HTTPS URLの重複定義（varsで適切にオーバーライド済み）
- **macOSシェル設定**: /etc/shellsへのfish追加とデフォルトシェル変更時のハング（条件付き実行で解決済み）

## 前提条件

- **macOS**: Homebrewのインストールが必要（未インストールの場合は手順が表示されます）
- **Git SSH鍵**: SSHでdotfilesをクローンする場合のみ必要（デフォルト）
- **すべてのプラットフォーム**: sudo権限が必要

## コマンド

### Playbookの実行

```bash
# macOS/Ubuntu用（自動検出）
ansible-playbook site.yml --ask-become-pass

# プラットフォーム別の直接実行も可能
ansible-playbook macos.yml --ask-become-pass
ansible-playbook ubuntu.yml --ask-become-pass

# Cloud Shell用
ansible-playbook cloudshell.yml --ask-become-pass

# デバッグ実行（変更内容を確認）
ansible-playbook <playbook>.yml --check --diff

# 詳細ログ付き実行
ansible-playbook <playbook>.yml -vvv --ask-become-pass
```

### ロールのテスト

```bash
# macOSロールのテスト
ansible-playbook roles/macos/tests/test.yml

# Ubuntuロールのテスト
ansible-playbook roles/ubuntu/tests/test.yml
```

### 新規ロールの作成

```bash
ansible-galaxy init --init-path=roles <ロール名>
```

### 構文チェック

```bash
ansible-playbook --syntax-check <playbook>.yml
```

## アーキテクチャ

リポジトリはAnsibleの標準的なロールベース構造に従っています:

```
.
├── roles/
│   ├── common/        # 共通タスクと設定
│   ├── cloudshell/    # Cloud Shell環境セットアップ
│   ├── macos/         # macOS環境セットアップ
│   └── ubuntu/        # Ubuntu環境セットアップ
├── *.yml              # プラットフォーム別のplaybook
└── site.yml           # メインエントリーポイント（プラットフォーム自動検出）
```

各ロールの内容:
- `tasks/main.yml`: 主要な自動化ロジック
- `vars/main.yml`: プラットフォーム固有のパッケージリストと設定
- `tests/test.yml`: ロールテスト用playbook（macos/ubuntuのみ）
- `defaults/main.yml`: デフォルト変数（commonロールで活用）
- `handlers/`, `meta/`: 現在は未使用

## 主要なプロビジョニングタスク

共通ロール（common）が実行する機能:
1. **Git email設定**: 初回実行時にプロンプトで入力、以降はキャッシュ
2. **開発ディレクトリ**: `~/dev/workspace`と`~/dev/readonly`の作成
3. **Git基本設定**: user.name、user.email、エディタなどの設定
4. **Dotfiles管理**: dotfilesリポジトリのクローン、更新、setup.shの実行

各プラットフォームロールが実行する機能:
1. **パッケージインストール**: プラットフォーム固有のツール
2. **シェル環境**: Fishシェルとプラットフォーム別の設定
3. **プラットフォーム固有の設定**: macOSデフォルト、権限設定など

## プラットフォーム別の詳細

### macOS
- **パッケージマネージャー**: Homebrew（要事前インストール、チェック機能あり）
- **シェル設定**: oh-my-zsh → Fish（/opt/homebrew/bin/fish - Apple Silicon前提）
- **特有の設定**: macOSシステムデフォルト、MacVimインストール、credential.helper設定、Starship（Homebrew経由）、AWS Session Manager Plugin（Homebrew Cask経由）
- **注意点**: Intel Macでは/usr/local/bin/fishパスの調整が必要な場合あり

### Ubuntu
- **パッケージマネージャー**: APT、Snap（jump、Starship用）
- **シェル設定**: 直接Fishに切り替え（oh-my-fish未使用）
- **特有の設定**: 開発ツールの豊富なセット、Terraformインストール（HashiCorp公式リポジトリ）、1Password CLI、Starship（Snap経由）、AWS Session Manager Plugin（公式.debパッケージ）

### Cloud Shell
- **パッケージマネージャー**: YUM
- **シェル設定**: oh-my-fish + robbyrussellテーマ、.bashrc経由でFish起動
- **特有の設定**: HTTPSでのdotfilesクローン、pecoの手動インストール、Starship（スクリプトインストール）、AWS Session Manager Plugin（公式.rpmパッケージ）
- **注意点**: デフォルトシェル変更不可（.bashrcでFish起動）

## トラブルシューティング

### よくある問題と対処法

1. **SSH鍵エラー（dotfilesクローン時）**
   ```bash
   # SSH鍵が設定されていない場合
   ssh-keygen -t ed25519 -C "your-email@example.com"
   # GitHubに公開鍵を追加
   ```

2. **macOSでのFishパスエラー**
   ```bash
   # Intel Macの場合、Fishのパスを確認
   which fish
   # 必要に応じてtasks/main.ymlのパスを修正
   ```

3. **権限エラー**
   ```bash
   # sudo権限が必要な場合
   ansible-playbook <playbook>.yml --ask-become-pass
   ```

## 開発時の推奨事項

1. **部分実行**: 現在タグは未定義のため、特定タスクのみ実行したい場合はタスクにタグを追加
2. **冪等性の確認**: 複数回実行しても安全か確認（userモジュールによるシェル変更など実装済み）
3. **エラーハンドリング**: dotfiles setup.shなど重要なタスクには適切なエラー処理を実装
4. **新規ツールの追加**: `roles/<プラットフォーム>/vars/main.yml`に追加してパッケージマネージャー経由でインストール
5. **Go言語**: 必要に応じて手動インストール（自動化からは除外）

## 開発・運用

### コミット規則
このプロジェクトでは以下のコミット規則に従います：

- **❌ 禁止**: Co-Authored-By行の追加
- **❌ 禁止**: "🤖 Generated with Claude Code" フッター  
- **❌ 禁止**: コミットメッセージでのClaude言及