---
title: "https対応node.jsの試行のためのEC2インスタンス作成"
templateKey: blog-post
date: 2020-07-15T17:35:00+09:00
tags: ["node.js", "Let's Encript"]
draft: true
---

````bash
https://docs.aws.amazon.com/ja_jp/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html

ssh -i ~/.ssh/mbp-matsuda-t.pem ec2-user@xxx.yyy.zzz.aaa

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install node
node -e "console.log('Running Node.js ' + process.version)"


# 自己証明証明書(オレオレ証明書)の作成
https://qiita.com/_daisuke/items/0ad521a2c4e87cafc09e
openssl genrsa -aes128 4096 > server.pem
Enter pass phrase:[パスフレーズを入力]
Verifying - Enter pass phrase:[パスフレーズの再入力]

chmod 400 server.pem
openssl req -new -key server.pem > server.csr

Country Name (2 letter code) [XX]:[国コード、今回はJPを入力]
State or Province Name (full name) []:[都道府県名]
Locality Name (eg, city) [Default City]:[市区町村名]
Organization Name (eg, company) [Default Company Ltd]:[企業名、組織名]
Organizational Unit Name (eg, section) []:[部署名]
Common Name (eg, your name or your server's hostname) []:[FQDNホスト名　例：www.sample.jp]
Email Address []:[Eメールアドレス（担当者）]

chmod 400 server.csr

openssl x509 -in server.csr -days 365 -req -signkey server.pem > server.crt

chmod 400 server.crt

openssl rsa -in server.pem -out server.rsa

chmod 400 server.rsa



# Let's Encriptの証明書作成(ウェブサーバなし、Amazon Linux 2)
https://certbot.eff.org/lets-encrypt/centosrhel7-other
※上の手順ではchatbotインストールできなかったため  https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html


※ポート80のインバウンドを有効にする。

sudo wget -r --no-parent -A 'epel-release-*.rpm' http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/

sudo rpm -Uvh dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-*.rpm

sudo yum-config-manager --enable epel*

# epelの夕刻確認
sudo yum repolist all
.

sudo yum -y install certbot

# コマンドが一時的ウェブサーバを起動するsudo certbot certonly --standalone

Enter email address (used for urgent renewal and security notices) (Enter 'c' to cancel): ikomiki@gmail.com

(A)gree/(C)ancel: A

（メールアドレスを共有してよいか？）
(Y)es/(N)o:n

Please enter in your domain name(s) (comma and/or space separated)  (Enter 'c' to cancel): ikomiki.com

出力から証明書とチェーン、キーファイルを取得する
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/ikomiki.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/ikomiki.com/privkey.pem

# cronを設定する。ウェブサーバがない前提
echo "0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && certbot renew" | sudo tee -a /etc/crontab > /dev/null



sudo setfacl -R -m u:ec2-user:rX /etc/letsencrypt/
# 再起実行、設定

------
https://qiita.com/_daisuke/items/0ad521a2c4e87cafc09e

```www.js
/*
 * https1.js
 * Copyright (C) 2014 kaoru <kaoru@bsd>
 */
var https = require('https');
var fs = require('fs');
var ssl_server_key = 'server.rsa';
var ssl_server_crt = 'server.crt';
var port = 8443;

var options = {
        key: fs.readFileSync(ssl_server_key),
        cert: fs.readFileSync(ssl_server_crt)
};

https.createServer(options, function (req,res) {
        res.writeHead(200, {
                'Content-Type': 'text/plain'
        });
        res.end("Hello, world\n");
}).listen(port);
````

node www.js

別ブラウザで https://サーバ名:8443/ を開く
警告を無視して開く

```

```
