# jenkinsOnDocker

docker-compose.yml
~~~
version: '3'
services:
  jenkins:
    container_name: jenkins
    image: jenkins:latest
    volumes:
      - ./jenkins_home:/var/jenkins_home
    ports:
      - "8080:8080"
    restart: always
 ~~~
 
 
 ■db2クライアントのDLページ
https://www-01.ibm.com/support/docview.wss?rs=71&uid=swg21385217

■jenkinsコンテナにログイン
sudo docker exec -it -u root jenkins bash

■vimのインストール
apt-get update
apt-get install vim

■sudoのインストール　設定(jenkinsユーザのsudo時にパスワードをもとめない）
http://pogin.hatenablog.com/entry/20111008/1318101299

apt-get install sudo
visudo

~~~
%sudo   ALL(ALL:ALL) ALL
jenkins ALL=(ALL) NOPASSWD:ALL
~~~

■db2グループ・ユーザの追加
groupadd db2
useradd -m db2 -g db2
passwd db2

■グループにユーザが追加されているか確認
groups db2


■インストーラーの解答
tar xvzf ibm_data_server_client_linuxx64_v11.5.tar.gz

■インストール
./db2_install -f sysreq -p client -c /root/work_db2client/client/nlpack/ -L JP

■インスタンスの作成
/opt/ibm/db2/V11.5/instance/db2icrt -s client db2

■db2ユーザに変更
sudo su - db2

■文字化け？
export DB2CODEPAGE=943

■カタログ
db2 catalog tcpip node sampledb remote 192.168.100.2 server 50000

db2 list node directory

db2 catalog database sample at node sampledb

db2 connect to sample user db2inst1 using password

■Jenkinsからの実行用SH
db2.sh
~~~
#!/bin/bash

sudo su - db2 <<EOF
db2 connect to sample user db2inst1 using password
EOF
~~~

■.profileに環境変数の設定をしておく
export DB2CODEPAGE=943

■JenkisDockerイメージのjenkins実行ファイルの場所。ここにJenkisのwarがある
/usr/share/jenkins

■Jenkinsをバージョンアップするときは上記フォルダのwarを上書きする。上書き前にかならずバックアップをとっておく。
cp /var/jenkins_home/jenkinsupdatework/jenkins.war /usr/share/jenkins/jenkins.war

■作成したimageを保管しておく
sudo docker commit -m "環境の説明とかを書きます" コンテナID イメージ名

■イメージが保管されたかを確認する
sudo docker images



