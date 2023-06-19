# homelab/raspberrypi
ラスパイについてのまとめ。
### 操作
| 操作 | コマンド |
| :------ | :------ |
| ssh | `ssh -p [ポート番号] [ユーザー名]@[IPアドレス]` |
| gitプル | `git pull origin main` |
| gitクローン | `git clone [URL。httpsもsshも]` |
| 再起動 | `sudo reboot` |
| シャットダウン | `sudo shutdown -h now` |
| apt-getの | `sudo apt-get update` |
|  | `sudo apt-get upgrade` |
| apache2再起動 | `sudo reboot apache2 restart` |
| tomcat9再起動 | `sudo service tomcat9 restart` |

###よく使う場所
| サービス | パス |
| :------ | :------ |
| apache | /var/www/html/ |
| tomcat | /var/lib/tomcat9/webapps/ |

## ラズパイ設定

### 定番初期設定
https://qiita.com/tksnkym/items/31a237e27cbc51790cdd
https://qiita.com/c60evaporator/items/ebe9c6e8a445fed859dc
https://qiita.com/picker/items/0466c55f9a03ebb0bd90 ラズパイ起動からApache導入まで

### piユーザー削除
piがログインし続けられてて消せないこともあって、ログインできなくしてから消したらいけました。
https://www.mikan-tech.net/entry/raspi-remove-default-user

### rootユーザーのパスワード変更
### ユーザー追加
### sshポートの変更
### rootユーザーでのログイン
prohobit-passwordをnoへ。
### ufwのポート解放
### キーボードの設定
### sshについて
ラズパイと公開鍵でのssh。やってない気がする。
https://tool-lab.com/raspi-key-authentication-over-ssh/

### SDカード
SDカードが壊れやすいので対策できるよって話。まだやってない。
https://iot-plus.net/make/raspi/extend-sdcard-lifetime-5plus1/

## Webサーバー設定

### ufw
https://raspida.com/firewall4raspbian-ufw
```
sudo apt install ufw
```

### vim

https://qazsedcftf.blogspot.com/2019/12/raspberry-pi-vim.html
```
sudo apt-get install vim
```

### vnc

https://denseforestreviewcannotsay.blogspot.com/2019/12/mac-raspbianvnc-apple-remote-desktop.html

### java8

```
sudo apt-get install openjdk-8-jdk
```

### apache2

```
apt-get install apache2
```

### tomcat9

https://tony1124.hatenablog.com/entry/2019/08/23/121942
(正式版じゃないので、やりかたを変えるべき。現状めんどくさいのでこれでやってる。)
```
sudo apt install tomcat9 tomcat9-admin tomcat9-docs tomcat9-examples
```
正式版うんぬんについて
aptだと正式版ではないらしく、以下では足りない。
https://qiita.com/ekzemplaro/items/b6dd39daeb027227234d
でも、以下ならちゃんと全部入ってそう
https://tony1124.hatenablog.com/entry/2019/08/23/121942

### ajp

https://qiita.com/t-kasa-moto/items/781b15f9903edac78269
server.xmlの8080のとこ、サイトと違うけどコメントアウトした。apacheのデフォのconfが動いてて永遠にtomcat_ajp.confが働かんかった。消すか000-default.confirmの中身コメントアウトするといいよ。

### php

```
sudo apt install php libapache-mod-php -y
```

### MariaDB

https://www.raspberrypirulo.net/entry/mariadb-install
https://takezou2k.myds.me/wp/raspberrypi-add-mariadb/

```
sudo apt-get install mariadb-server
```

ROOTのパスワード変更。mysql_secure_installationでもできなかった時があったけど、何があったんだろうか、今はできました。
https://ja.stackoverflow.com/questions/78148/mysqlにパスワード無しでログインできてしまうのはなぜ
https://mariadb.com/kb/ja/mysql_secure_installation/

```
ALTER USER 'root'@'localhost' identified BY '*****';
```

### vsftpd
後述のsambaでいいので、いらない気がします。
https://www.kunimiyasoft.com/raspberrypi-vsftpd/

ftpサーバーのvsftpdのconfファイルについて
https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/6/html/deployment_guide/s2-ftp-vsftpd-conf

```
sudo apt-get install vsftpd
```

### samba
https://tool-lab.com/raspberrypi-startup-14/#index_id2
共有フォルダのパス設定など
https://toki-blog.com/pi-samba/

```
sudo apt-get -y install samba
```

### git

```
sudo apt-get install git-all
```

クローンの設定。sudoつけて鍵作るだか、cloneするだかして一生上手くいかなかった。鍵の所有者の問題だと思うので気をつけて。
https://qiita.com/shizuma/items/2b2f873a0034839e47ce

### Homebridge
https://otaku-ringo.com/how-to-install-homebridge-for-beginners/#toc6

インストール後、homebridgeのページを開く際は、ポートは8581のようです。

### Nature Remo
https://otaku-ringo.com/homebridge-natureremo-homekit/

再起動の方法
https://simple-was-best.com/raspberry-homebridge-reboot/

#### Nature Remo テレビ
Homebridgeの再起動の度にポートが変わっちゃうので、やるならufwをオフにするか、他の方法を探すこと。
https://github.com/sskmy1024y/homebridge-nature-remo-tv-remote/blob/main/README.ja.md

### avahi-daemon
すでに入ってるもの。

再起動などのメモ。
https://qiita.com/takish/items/1a640576caa2d0dfefc4

## 疑問
Tomcatのwebapp下に直接.warの中身とか.class達おいても実行できない理由は？(.warしてtomcatに展開させないとダメな理由)
参考かわからん：https://www.itmedia.co.jp/enterprise/0212/09/epn14.html
https://gakumon.tech/tomcat/server_xml/host.html#automatic_application_deployment

sourcetreeでbinフォルダ下がアップできない謎。