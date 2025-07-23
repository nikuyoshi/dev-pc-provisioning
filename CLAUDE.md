# CLAUDE.md

このファイルは、このリポジトリでコード作業を行う際のClaude Code (claude.ai/code) への指針を提供します。

## リポジトリ概要

これは、macOS、Ubuntu、Cloud Shellプラットフォーム向けの開発環境セットアップを自動化するAnsibleベースのプロビジョニングリポジトリです。Ansibleロールを使用して開発ツールのインストール、シェルの設定、一貫した開発環境の構築を行います。

## 重要な注意事項

このリポジトリは個人用（Hiroki Uchida）の開発環境セットアップ用です。Git設定やdotfilesリポジトリURLなどの個人設定は意図的にハードコードされています。

### 既知の問題
- **macOSロール**: oh-my-zshのインストールタスクが3回重複しています（roles/macos/tasks/main.yml）
- **site.yml**: ubuntu.ymlのみをインポート（macOSユーザーは直接macos.ymlを実行する必要があります）
- **macOSテスト**: test.ymlが`mac`ロールを参照していますが、実際のロール名は`macos`です

## 前提条件

- **macOS/Ubuntu**: GitのSSH鍵設定が必要（dotfilesクローン用）
- **すべてのプラットフォーム**: sudo権限が必要

## コマンド

### Playbookの実行

```bash
# macOS用（site.ymlは使用しないこと）
ansible-playbook macos.yml --ask-become-pass

# Ubuntu用
ansible-playbook ubuntu.yml --ask-become-pass
# または
ansible-playbook site.yml --ask-become-pass

# Cloud Shell用
ansible-playbook cloudshell.yml --ask-become-pass

# デバッグ実行（変更内容を確認）
ansible-playbook <playbook>.yml --check --diff

# 詳細ログ付き実行
ansible-playbook <playbook>.yml -vvv --ask-become-pass
```

### ロールのテスト

```bash
# macOSロールのテスト（注意：現在動作しません - ロール名の不一致）
# ansible-playbook roles/macos/tests/test.yml

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
│   ├── cloudshell/    # Cloud Shell環境セットアップ
│   ├── macos/         # macOS環境セットアップ
│   └── ubuntu/        # Ubuntu環境セットアップ
├── *.yml              # プラットフォーム別のplaybook
└── site.yml           # メインエントリーポイント（ubuntu.ymlのみインポート）
```

各ロールの内容:
- `tasks/main.yml`: 主要な自動化ロジック
- `vars/main.yml`: プラットフォーム固有のパッケージリストと設定
- `tests/test.yml`: ロールテスト用playbook（macos/ubuntuのみ）
- `defaults/`, `handlers/`, `meta/`: 現在は未使用

## 主要なプロビジョニングタスク

すべてのロールが実行する主要機能:
1. **Git設定**: ユーザー認証情報と設定の構成
2. **開発ディレクトリ**: `~/dev/workspace`と`~/dev/readonly`の作成
3. **パッケージインストール**: プラットフォーム固有のパッケージマネージャーによる開発ツールのインストール
4. **シェル環境**: Fishシェルの設定とフレームワークのインストール
5. **Dotfiles管理**: 個人用dotfilesリポジトリのクローンと管理
6. **プロンプトのカスタマイズ**: Starshipプロンプトのインストール

## プラットフォーム別の詳細

### macOS
- **パッケージマネージャー**: Homebrew（自動インストール）
- **シェル設定**: oh-my-zsh → Fish（/opt/homebrew/bin/fish - Apple Silicon前提）
- **特有の設定**: macOSシステムデフォルト、MacVimインストール
- **注意点**: oh-my-zshタスクの重複、Intel Macでのパス問題の可能性

### Ubuntu
- **パッケージマネージャー**: APT、Snap（jumpツール用）
- **シェル設定**: 直接Fishに切り替え（oh-my-fish未使用）
- **特有の設定**: 開発ツールの豊富なセット

### Cloud Shell
- **パッケージマネージャー**: YUM
- **シェル設定**: oh-my-fish + robbyrussellテーマ、.bashrc経由でFish起動
- **特有の設定**: HTTPSでのdotfilesクローン、pecoの手動インストール
- **注意点**: .bashrcの完全上書き、デフォルトシェル変更不可

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
2. **冪等性の確認**: 複数回実行しても安全か確認（特にシェルコマンド実行部分）
3. **エラーハンドリング**: 重要なタスクには`ignore_errors: no`を明示的に設定
4. **新規ツールの追加**: `roles/<プラットフォーム>/vars/main.yml`に追加してパッケージマネージャー経由でインストール

## 開発・運用

### コミット規則
このプロジェクトでは以下のコミット規則に従います：

- **❌ 禁止**: Co-Authored-By行の追加
- **❌ 禁止**: "🤖 Generated with Claude Code" フッター  
- **❌ 禁止**: コミットメッセージでのClaude言及