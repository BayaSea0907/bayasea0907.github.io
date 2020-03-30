# Railsサーバーにアクセスするまで


***

### AWSで構築

* ①の環境構築終わった後、③DB構築を行う

* WEBサーバーを起動
  `bundle exec rails server`

***

### VMWareで構築

※ 参考URL:
https://eng-entrance.com/linux-centos-port
https://qiita.com/NaokiIshimura/items/888d77f924877e5dc5e0

----------------------------------------------------------------------------------------
◇ 目次

1. nginx導入
2. DBを準備
3. WEBサーバーを起動

----------------------------------------------------------------------------------------
◇ nginxの導入

* インストールする
  `sudo yum install -y nginx`

* nginxを起動する
  `systemctl start nginx`

* ファイアーウォールを停止する
  `systemctl stop firewall.d`

* ローカルIPでnginxの画面に入れるようになる
  http://xxx.xxx.xxx.xxx

----------------------------------------------------------------------------------------
◇　DBを準備

* DBサーバー起動に必要なパッケージをインストールする
  `yum install -y mysql2 openssl-devel`

* DBを用意する

* rails_project/config/database.ymlを設定する

→ 続きは、環境構築③_DB構築を見る。

----------------------------------------------------------------------------------------
◇ WEBサーバーを起動

* gemをインストールする。
  `bundle install`

* サーバーを起動する
  bundle exec rails server -p 7777(デフォルトだと3000) -b 0.0.0.0(bind。すべてのローカルIPを許可)
