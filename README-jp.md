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
- DigiCert
- セコム
- UPKI電子証明書発行サービス



### WebIP証明書の実在調査




## 結果

### 主要なWebサーバ証明書発行サービスの規定調査

### WebIP証明書の実在調査


## 発表
金岡 晃、小山 裕輝、岡田 雅之：IPアドレスをサブジェクトに含んだWebサーバ証明書の調査と分析、2022年暗号と情報セキュリティシンポジウム（SCIS2022）、2022

## Contact
金岡 晃（東邦大学）
akira.kanaoka@is.sci.toho-u.ac.jp

## 謝辞
本研究の一部はJSPS科研費 JP19K11972の助成を受けたものです。
