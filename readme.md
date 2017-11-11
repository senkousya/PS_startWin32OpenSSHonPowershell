# 🔰PowershellでWin32-OpenSSHを利用してSSH接続をする

## 🔰Win32-OpenSSH公式サイト

- [github -Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH)

## 🔰概要

PowershellでSSH接続は標準機能でサポートしていない訳ですが。

マイクロソフトがリリースしているWin32-OpenSSHを利用するとPowershellでSSH接続が出来るようになるので試してみました。

## 🔰作業環境

- Windows10
- PSVersion 5.1.15063.483

## 🔰Win32-OpenSSHのインストール

- [Install Win32 OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)
- [chocolatey - Win32 OpenSSH](https://chocolatey.org/packages/openssh)

公式サイトのインストール手順に沿ってインストール。

もしくはchocolateyにWin32 OpenSSHのパッケージがあるのでお好きな方法でインストールして下さい。

## 🔰SSH接続してみる

### 🔰SSH接続に利用するキーペアの登録を行う。

▶AWS EC2コンソールの中にあるキーペアを選択する。  
![](image/aws.console.create.keypair.step001.png)

▶キーペアの作成を選択する。  
![](image/aws.console.create.keypair.step002.png)

▶キーペア名を入力して作成を選択する。  
![](image/aws.console.create.keypair.step003.png)

▶キーペアが登録されたのが確認できる。  
![](image/aws.console.create.keypair.step004.png)

キーペアが登録され、同時にpemファイルがダウンロードされる。
このpemファイルは再取得は出来ないので大切に保管しましょう。

▶中身はテキストエディタで開くとこんな感じのファイルとなっている。  
![](image/pemfile.png)

### 🔰接続先としてAWS EC2でLinuxインスタンスを立ててみる

SSHの接続先としてAWSで適当なLinuxインスタンスを立ち上げて作成したキーペアで接続テストしてみる。

▶AWSコンソールのEC2からインスタンスの作成  
![](image/create.instance.step001.png)

▶今回はAmazonLinuxを選択  
![](image/create.instance.step002.png)

▶セキュリティグループはSSHが接続できる設定にする  
![](image/create.instance.step003.png)

SSH接続に利用するpemファイルを指定する

**既存のキーペアの選択**から先程登録したキーペアを選択する。

▶新しいキーペアの作成を選択すれば、この画面からキーペアを作成出来たりもする。  
![](image/create.instance.step004.png)

▶EC2インスタンスが立ち上がるのを確認  
![](image/create.instance.step005.png)

### 🔰PowershellからSSHで接続

インスタンスを立てる時に指定したpemファイルでSSH接続を行う。

なお接続方法についてはAWSのLinuxインスタンスへの接続等に記載があります。

もしくはEC2のコンソールでインスタンスを選択してから接続ボタン or 右クリック -> 接続　で表示される情報を参照。

- [Linuxインスタンスへの接続](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/AccessingInstances.html?icmpid=docs_ec2_console)

▶右クリック -> 接続でインスタンスへの接続情報が表示される。  
![](image/connect.ssh.ec2.step001.png)

▶インスタンスへの接続情報の確認
![](image/connect.ssh.ec2.step002.png)

```powershell

ssh -i pemファイル ユーザ@host

```

上記のような感じでPowershellでsshコマンドの実行

ちなみに**ec2-user**はAmazonLinuxのデフォルトシステムユーザです。[Linux インスタンスでのユーザーアカウントの管理より](http://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/managing-users.html)  
![](image/connect.ssh.ec2.step003.png)

あっさりつながりました。

## 🔰総評

普段Powershellを利用して作業している時に、さっくりSSHで接続できるのは便利かもしれない。

あとローカルPCのPoewrshellスクリプト内でsshでリモートコマンドを実行するなんて使い方もいいのかもしれない。
