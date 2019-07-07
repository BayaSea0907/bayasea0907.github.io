# ④SSLの設定

※ SSL(HTTPSのS。現在はTLSという名称になっているが、SSLと呼び続けている)
※ 通信を暗号化して、セキュリティーを強化できる。
※ サイト運営者であることの証明ができる。

----------------------------------------------------------------------------------------
◇ SSLの設定

◆ open_sslをインストールする
sudo yum install openssl-devel

◆ ビルドする
・「./configure」

※ オプションを付けてビルドする場合
    --sbin-path=/usr/local/nginx/nginx	

　　などなど。nginxなど、バージョンによって依存モジュール(オプションで追加)が必要になる。
　　downloadページなどに記載されている。
　　
・configureに成功したら、「make」→「make install」を行う。


◆ 公開鍵暗号(crt)、証明書(csr)、秘密鍵暗号(.key)を作成する。

※ 作成コマンド
・openssl genrsa -out /ssl/server.key 2048 		← 秘密鍵暗号(.key)

・openssl req -new -key server.key > server.csr 	← 証明書(csr)
→ JP, Tokyo, Shibya-ku, ... 
   Common Name (eg, your name or your server's hostname) []:bayabayasea.com　 ※ 必ずhostsに書いたdomain名(localHost)を書く。

・openssl x509 -in server.csr -days 90 -req -signkey server.key > server.crt	← 公開鍵暗号(crt)


◆ ドメインの設定をする
・「C:\Windows\System32\drivers\etc\hosts」ファイルを、
　  メモ帳から"管理者権限"で編集する。

→ 192.168.56.2			localhost
(sshでログインしているIP)     (ドメイン名。bayasea.comとか)



◆ nginxに、sslの設定を追加する
→「/usr/local/nginx/conf/nginx.conf」の、下記の部分。
http {
	～
	server {
		listen 		443 ssl; ←これを書く。	TLS(SSL)は443で、デフォルト(http)は80。
		server_name	bayasea.com(ドメイン名。)

		ssl_cartificate		/etc/httpd/conf/ssl/server.crt;		公開鍵暗号
		ssl_cartificate_key	/etc/httpd/conf/ssl/server.key;		秘密鍵暗号
					↑
					任意のパス。	
	}
}
