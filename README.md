# dotfiles-pixel

# Android Native Linux Terminal Setup Guide

Android 15 (Pixel) から導入された純正のLinuxターミナルアプリ（Debian VM）を有効化し、セットアップする手順です。

## ⚙️ 有効化手順 (Activation Steps)

### 1. 開発者向けオプションを有効にする
まだ有効にしていない場合は、以下の手順で行います。

1.  Androidの **「設定」>「デバイス情報」** を開く。
2.  最下部の **「ビルド番号」** を **7回連続でタップ** する。
3.  パスコード（PIN）を入力すると有効化されます。

### 2. Linux開発環境をONにする
1.  **「設定」>「システム」>「開発者向けオプション」** を開く。
2.  リストの中から **「Linux 開発環境」** (Linux development environment) を探す。
3.  スイッチを **ON** に切り替える。

### 3. ターミナルアプリのセットアップ
1.  アプリドロワー（アプリ一覧）に **「Terminal」** というアイコンが追加されているのでタップして起動する。
2.  初回起動時、Debianのイメージファイルのダウンロードとセットアップが走るので待機する。

---

## 🚀 初期セットアップ (Initial Setup)

ターミナルが起動したら、まずはパッケージリストの更新と必須ツールのインストールを行います。
※ 純正ターミナルでは `sudo` が必須です。

```bash
# パッケージリストの更新とアップグレード
sudo apt update && sudo apt upgrade -y

# 必須ツールのインストール
sudo apt install -y curl git
```

---

## 🛠️ dotfilesのセットアップ (Dotfiles Setup)

### 1. SSH鍵の生成とGitHubへの登録
リポジトリをクローンするために、まずSSH鍵を生成してGitHubに登録します。

```bash
# SSH鍵の生成（質問にはすべてEnterでOK）
ssh-keygen -t ed25519 -C "pixel-android"

# 公開鍵の表示
cat ~/.ssh/id_ed25519.pub
```
表示された公開鍵（`ssh-ed25519`で始まる文字列）をコピーし、[GitHubのSSH Keys設定ページ](https://github.com/settings/keys)に登録してください。

### 2. リポジトリのクローン
ホームディレクトリに移動し、このリポジトリをクローンします。

```bash
cd ~
git clone git@github.com:nishioka-shinji/dotfiles-pixel.git
```

### 3. miseのセットアップ
[mise](https://mise.run/) を使って、各種ツールのバージョン管理とインストールを行います。

```bash
# miseのインストール
curl https://mise.run | sh

# .bashrcにmiseの起動設定を追記
echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bashrc

# 設定ファイルのシンボリックリンクを作成
mkdir -p ~/.config/mise
ln -sf ~/dotfiles-pixel/mise/mise.toml ~/.config/mise/config.toml

# .bashrcの変更を現在のシェルに反映
source ~/.bashrc

# 設定ファイルに基づきツールを一括インストール
mise install
```

### 4. セットアップの確認
`source ~/.bashrc` を実行したため、現在のセッションは既に `mise` が有効になっています。
以下のコマンドを実行し、バージョン情報が表示されれば `mise` のセットアップは成功です。

```bash
mise --version
```
