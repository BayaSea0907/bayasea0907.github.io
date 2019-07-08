# ① AWS RedHat Enterprise Linux 
※ これめちゃめちゃ分かりやすい。
https://www.adoc.co.jp/blog/e000289.html

----------------------------------------------------------------------------------------
◇ 目次

1. 環境構築の準備(AWS RedHat Enterprise Linux)
2. Rbenv導入
3. Ruby導入
4. Rails導入
5. Git導入
6. Railsサーバー...起動したけど開けない。環境構築②へ。

----------------------------------------------------------------------------------------
◇ 環境構築の準備(AWS RedHat Enterprise Linux ) 

1. rootログイン
   - ssh root@localhost -y

2. sudoの設定
   - 「visudo」を実行
   - 「root ALL=(ALL) ALL」の下あたりに、「user_name ALL = (ALL) ALL」を追加。

3. yumの更新、net-toolsのインストール
   - `sudo yum update  -y` (※ すべてのパッケージをアップデート。動作中のプログラムにまで影響を与え、予期しないエラーが発生することがある)
   - `sudo yum install -y perl net-tools`

----------------------------------------------------------------------------------------
◇ Rbenvの導入 (rubyのバージョン管理パッケージ)

※ これを参考にする。
https://qiita.com/ksugawara61/items/e3bb87d5e0dd49d20c8f


1. 必要パッケージをインストール
   - `sudo yum install -y git gcc bzip2 pcre pcre-devel openssl openssl-devel readline-devel telnet libaio zlib-devel`
   (※「sudo yum search pcre」とかもできます。)

2. rbenvのインストール (Githubからインストール)
   - `git clone https://github.com/rbenv/rbenv.git ~/.rbenv`

3. 「~/.bash_profile」にrbenvのパスを追加。
   - `echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile`
   - `echo 'eval "$(rbenv init -)"' >> ~/.bash_profile`
   - `source ~/.bash_profile`  (再読み込みで設定反映)

4. rbenvのバージョンを確認
   - `rbenv --version`


----------------------------------------------------------------------------------------
◇ Rubyの導入


1. ruby-buildプラグインの追加
   - `git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/pulugins/ruby-build`

2. rbenvのinstallコマンドを追加
   - `sudo ~/.rbenv/pilugins/ruby-build/install.sh`

3. rubyのインストール
   - `rbenv install -l`    で、インストール可能なrubyのversionを確認
   - `rbenv install 2.6.3` で、使用するversionをインストール(※ 2019/7/9現在の安定バージョン2.6.3)
   - `rbenv global 2.6.3`  で、環境全体でのrubyversionを指定

4. gemコマンドなどを利用できるように設定
   - `rbenv rehash`

----------------------------------------------------------------------------------------
◇ Railsの導入

1. gem install --no-rdoc rails

2. gem install bundler

3. rbenv rehash

4. rails -vで導入されていればOK
   - rails new project_name --skip-bundleでプロジェクト作成。

----------------------------------------------------------------------------------------
◇ Gitの導入

1. ターミナル上で、公開鍵、秘密鍵を生成
   - ssh-keygen -t rsa
     - すべてenterで生成する。
     - ※ sudoは使わない。

2. githubに公開鍵(id_rsa.pub)を登録する
   - https://github.com/settings/ssh

3. 接続を確かめる。問題なければOK
   - ssh -T git@github.com


----------------------------------------------------------------------------------------
◇ Gitでプロジェクトを管理する。

1. Github上で、リポジトリを作成する。(※ README.mdを作ると、cloneして作業開始することになるので注意！)
   - 「https://github.com/BayaSea0907/web-chat.git」みたいなのが出来る。

2. ターミナル上で、プロジェクトを作成。
   - rails new project_name
   - cd project_name

3. プロジェクトにGitの設定をする。
   - git init
   - git add .
   - git commit . -m 'first commit'
   
   ※ repogitoryを登録する。
   - remote add origin https://github.com/BayaSea0907/web-chat.git

   - git push origin master (※ 次回からは、ブランチを切ってそっちをpushする。)


----------------------------------------------------------------------------------------
◇ Railsでサーバーを起動するまで (bundle exec rails s)


1.「bundle install」を実行する。
   ※ sqlite3がないって言われたら、下記を実行。
      - sudo yum search sqlite
      - sudo yum install sqlite -y


2.「bundle exec rails s」を実行する。

   ※ javascriptのruntimeがないって言われたら、下記を実行。
     - sudo yum install epel-release -y
     - sudo yum install nodejs --enablerepo=epel -y


   ※「undefined method 'load_defaults'」って言われたら、下記を実行。
