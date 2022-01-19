WebIP証明書証明書の調査と分析
====
English version of README is [here](/README.md)

Webサーバ証明書の設定項目のサブジェクト内のCN (Common Name) やSAN (Subject Altanative Name) にFQDN (Fully Qualified Domain Name) の代わりにサーバのIPアドレスが設定された証明書 (以後 WebIP証明書と呼ぶ) が存在する。WebIP証明書であれば、クライアントがIPアドレスでそのサーバにSSL/TLS接続しても正しい通信相手として検証される。認証局（Certificate Authority, CA) がWebIP証明書の発行をする場合、そのルールの中にIPアドレスの利用権や所有権をサーバやサーバ管理者が保持しているかの確認がされていない可能性がある。その結果、本来であればそのIPアドレスの所有権を持たない第3者がそのIPアドレスに対する証明書発行を受けられてしまう可能性がある。

こういったリスクがある中で、どの程度の発行が行われているかの調査についてはこれまでされてこなかった。そこで本研究では全IPv4アドレスに対してTLS接続をし証明書を取得し、WebIP証明書の存在状況や利用されるIPv4アドレスの特徴を調査した。

## 調査方法
### 主要なWebサーバ証明書発行サービスの規定調査
主要なWebサーバ証明書発行サービスにおいてCP/CPSの記載を調べ、WebIP証明書の発行が可能かどうか、またWebIP証明書発行時のIPアドレスの確認方法を調べる。

調査した証明書発行サービスは以下の5つとした。それぞれのCPやCPSが掲載されているURLも付記する
- Let's Encrypt
  - https://letsencrypt.org/documents/isrg-cp-v3.1/
- Sectigo
  - https://sectigo.com/uploads/files/Sectigo_CPS_v5_2_3.pdf
- DigiCert
  - https://www.digicert.com/content/dam/digicert/pdfs/legal/DigiCert-CPS-V.5.6.pdf
- セコム
  - https://www.secomtrust.net/service/pfw/support/repository.html
- UPKI電子証明書発行サービス
  - https://repo1.secomtrust.net/sppca/nii/odca4/index.html


### WebIP証明書の実在調査
WebIP証明書の実在調査は以下の段階により行った
1. 全IPv4アドレスのTCP443番ポート開放調査
2. 443ポート開放IPアドレスにhttp（TLS）アクセス
3. 取得証明書のSubject CN（Common Name）とSAN（Subject Alt Name）フィールドの確認

それぞれの詳細は以下の通りである。

#### 全IPv4アドレスのTCP443番ポート開放調査
Amazon Web Service上のEC2インスタンスより[ZMap](https://github.com/zmap/zmap)を用いてインターネット上のIPv4アドレスで443番ポートのサービスを提供しているホストを調査

#### 443ポート開放IPアドレスにhttp（TLS）アクセス
443番ポートを開放しているIPv4アドレスに対し、opensslのs_clientを用いてhttps接続を試みる。接続に10秒以上を要するものを除外した。

実際に利用したコマンドは以下の通りである。
    timeout 10s openssl s_client -connect $line:443






## 結果

### 主要なWebサーバ証明書発行サービスの規定調査
|発行者|記載有無|記載場所|記載内容|
----|----|----|----
|Let's Encrypt|あり|CP "3.2.2.5 Authentication for an IP Address"|- Webコンテンツに特定の情報を記載<br>- Email, Fax, SMSまたは郵便による確認<br>- DNS逆引きによる確認<br>- IPアドレス登録機関に登録した電話番号への電話<br>- ACME "http-01" による確認<br>- ACME "tls-alpn-01" による確認|
|Sectigo|あり|CPS "3.2.2.1.2 IP Address Verification"|- Webコンテンツに特定の情報を記載<br>- Email, Fax, SMSまたは郵便による確認<br>- DNS逆引きによる確認|
|DigiCert|あり|CPS "3.2.2.1 Verification of IP Address"|- Webコンテンツに特定の情報を記載<br>- Email, Fax, SMSまたは郵便による確認<br>- DNS逆引きによる確認<br>- IPアドレス登録機関に登録した電話番号への電話<br>- ACME "http-01" による確認<br>- ACME "tls-alpn-01" による確認|
|セコム|なし|---||
|UPKI|なし|---||



### WebIP証明書の実在調査


## 発表
金岡 晃、小山 裕輝、岡田 雅之：IPアドレスをサブジェクトに含んだWebサーバ証明書の調査と分析、2022年暗号と情報セキュリティシンポジウム（SCIS2022）、2022

## Contact
金岡 晃（東邦大学）
akira.kanaoka@is.sci.toho-u.ac.jp

## 謝辞
本研究の一部はJSPS科研費 JP19K11972の助成を受けたものです。

