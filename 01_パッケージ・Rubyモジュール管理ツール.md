# yum, rpm, gem, bundler, rbenvのメモ
※ 2018年新卒入社後、約1年Ruby, Ruby on Railsを使って覚えたこと  
参考: https://uxmilk.jp/9715  

***

### rpm (Red Hat Package Manager)
* linux系のパッケージインストールマネージャー
* 依存性の解決はできない (パッケージ単体でしかインストールされない)  
* rubyもパッケージ

***

### yum (Yellowdog Updater Modified)
* linux系のパッケージインストールマネージャー
* 依存性の解決ができる (パッケージに依存している、別のパッケージもインストールしてくれる)  
* 内部的にrpmを使用している

***

### rbenv
* rubyのバージョン管理ツール
* command
* * rbenv global versionで、ユーザー全体が使用するrubyのバージョンを設定
* * rbenv local versionで、カレントディレクトリ以下のバージョンを設定 (カレントディレクトリに.ruby-version ファイルを書き出す)

***

### gem
* rubyのライブラリ・モジュール
* rubyの本体に標準で付属している

***

### bundler
* gemのバージョン、依存関係を管理するgem
* カレントディレクトリのGemfileに記載された内容をもとに、install, updateを行う
