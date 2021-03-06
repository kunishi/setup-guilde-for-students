# ユースケース別プログラム開発環境セットアップ手順
 
主に自分が所属する研究室の学生向けに、自分の経験を基に書いたオススメ手順です。Creative Commonsで公開しますので、環境や好みに合わせて適宜書き換えて使用してください。
 
## 最初にパソコンをもらったら
 
プログラム開発目的であれば、まず以下のことをしましょう。これはプログラミング言語によらず、だいたい必要になります。

- ターミナルを使えるようにする
- パッケージ管理ソフトのインストール
-  SSHの設定、特に公開鍵暗号による認証の設定
-  ソフトウェアレポジトリサービスへの登録

### ターミナルを使えるようにする

IDE（Integrated Development Environment, 統合開発環境）などが便利になって、GUIベースでもかなりプログラム開発はできるようになりました。ですが、まだまだコマンドラインから実行すべき作業も発生するのが現状です。

#### Ubuntu Linux
標準デスクトップのメニューから実行できます。

#### MacOS X
ターミナルが /Applications/Utilities/Terminal.app にあります。日本語環境だと「アプリケーション→ユーティリティ→ターミナル」です。起動しやすいように、ドックに入れておくなり、デスクトップにショートカットを作るなりしておくと良いです。

ターミナルを立ち上げると bash が動いています。Linuxに慣れていれば、Unix由来のコマンドをLinuxとほぼ同じように使うことができます。

#### Windows
Windowsはやや独自です。標準のターミナル（コマンド・プロンプト）が使い勝手があまり良くないためか、サードパーティ製も含めて乱立状態で、なかなか決定打がありません。

ここでは、Windowsにあらかじめ入っていて、Microsoftがコマンド・プロンプトの後継と位置付けているPowerShellを想定します。Windows 10なら、スタートメニュー→All Appsの中から起動できます。これも、タスクバーに登録しておいて、簡単に起動できるようにしておくと良いでしょう。

なお、以下の説明では、ユーザ権限で起動する場合と管理者権限で起動する場合があります。

###   パッケージ管理ソフトのインストール

開発過程ではいろんなソフトを使います。エディタやIDE（統合開発環境）、諸々のサードパーティ製ライブラリなどなど。こういうのをいちいち、Webからインストーラをダウンロードして、手でクリックして実行、なんていうことをやっていくのはめんどくさい。こういう作業の手間を軽減してくれるのがパッケージ管理ソフトです。

#### Ubuntu Linux

OS標準で apt というパッケージ管理ソフトが用意されています。せっかくですので、練習も兼ねて、Cコンパイラなどの開発ツールをインストールしておきましょう。ターミナルで以下を実行します。

```
sudo apt-get install build-essential
```

#### MacOS X

今、MacOS Xで一番普及しているパッケージ管理ソフトは [Homebrew](http://brew.sh/index_ja.html) でしょう。ここではこれを使います。

HomebrewはMacOS Xの標準開発環境であるXcodeを必要とします。ターミナルから以下を実行しておきましょう。

```
sudo xcodebuild -license
```

Xcodeのライセンスに関する注意がいろいろ表示されて、最後に [agree, print, cancel] の何れかを入力させられます。”agree” を入力しましょう。

この後、Homebrewのサイトにあるインストール手順に従って作業を進めてください。日本語なので迷わないと思います。

標準のHomebrewで管理できるのはGUIを含まないアプリケーションだけですが、[Homebrew Cask](https://caskroom.io)というHomebrewの拡張機能を使うと、ダブルクリックして起動するようなアプリケーションも管理できます。以下をターミナルで実行してインストールしておきましょう。

```
brew tap caskroom/cask
```

#### Windows

Windowsではパッケージ管理ソフトもやや乱立気味なのですが、一番コミュニティが活発かつパッケージ数も多いのは[Chocolatey](https://chocolatey.org)です。これを使うのが良いと思います。

PowerShellを管理者権限で立ち上げて[^1]、Chocolateyのサイトにあるインストール手順に従ってインストールします。

[^1]: PowerShellのアイコンを右クリックすると、「管理者権限で実行」という項目があります。

### SSHの設定

サーバ管理には欠かせないSSH（Secure Shell）ですが、最近、GitHubなどのソフトウェアレポジトリサービスを快適に使う時にもSSHによる公開鍵認証が必要となっています。

SSHの公開鍵方式による認証では、以下の作業を行う必要があります。

- 事前準備
	- パソコン上で公開鍵と秘密鍵のペアを生成します。このとき、何らかの文字列（パスフレーズという）を入力する必要があります。
	- 公開鍵を、SSHでログインしたいサーバやサービスに登録します。
	- パソコン上で、ユーザに代わってぱすふれーずを自動入力してくれるプログラム（SSHエージェント）を常時起動するようにしておきます。
- サーバやサービスへの接続時
	- SSHでサーバやサービスに接続しようとすると、パスフレーズの入力を求められます。正しく入力すると、SSHでの接続ができます。
	- SSHエージェントが起動していると、ユーザの代わりにパスフレーズをSSHエージェントが自動で送信してくれるようになります。つまり、ユーザはいちいちパスフレーズを入力する必要がなくなります。

#### UbuntuおよびMacOS X

UbuntuおよびMacOS Xでは、OpenSSHというSSHの実装が標準で搭載されています。

