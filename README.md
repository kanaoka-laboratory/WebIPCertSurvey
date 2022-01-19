Web IP Certificate Survey
====
日本語版READMEは[こちら](/README-jp.md)

There exists a certificate whose server's IP address is set in CN (Common Name) in the Subject field or SAN (Subject Alternative Name)  field of a Web server certificate instead of FQDN (Fully Qualified Domain Name).
We call such certificates WebIP certificates.

With a WebIP certificate, a client's SSL/TLS connection to a server with an IP address will be verified as a valid communication partner. When a Certificate Authority (CA) issues a WebIP certificate, its rules may not check whether the server or server administrator retains the right to use or own the IP address. As a result, a third party who does not have ownership of the IP address may issue a certificate for that IP address.

Despite these risks, there has been no survey on how many certificates are issued. In this study, we obtained certificates through TLS connections to all IPv4 addresses and investigated the existence of WebIP certificates and the characteristics of IPv4 addresses used.

## Methodology

### Survey of Certificate Policy (CP) and Certification Practice Statement (CPS)  of Major Web Server Certificate Issuer
Check the CP/CPS descriptions of major Web server certificate issuing services to determine if they can issue WebIP certificates and how to check IP addresses when issuing WebIP certificates.

The following five certificate issuing services were surveyed.
The URLs where the CPs and CPSs of each of them are available are also appended.

- Let's Encrypt
  - https://letsencrypt.org/documents/isrg-cp-v3.1/
- Sectigo
  - https://sectigo.com/uploads/files/Sectigo_CPS_v5_2_3.pdf
- DigiCert
  - https://www.digicert.com/content/dam/digicert/pdfs/legal/DigiCert-CPS-V.5.6.pdf
- SECOM
  - https://www.secomtrust.net/service/pfw/support/repository.html
- UPKI Certificate Issuing Service
  - https://repo1.secomtrust.net/sppca/nii/odca4/index.html


### Survey of the Existence of Web IP Certificates
The existence of the WebIP certificate was surveyed in the following steps
1. Survey of TCP port 443 openings for all IPv4 addresses
1. https (TLS) access to IP address with port 443 open
1. Check the Subject CN and SAN fields of the acquired certificate

The details of each are as follows.


#### Survey of TCP port 443 openings for all IPv4 addresses
From the EC2 instance on the Amazon Web Service, use [ZMAP](https://github.com/zmap/zmap) to examine the hosts that provide services on port 443 using IPv4 addresses on the Internet.

#### https (TLS) access to IP address with port 443 open
Attempt to make an https connection using openssl s_client to an IPv4 address that has port 443 open. Those that took more than 10 seconds to connect were excluded.

The actual commands we used are as follows.

~~~
timeout 10s openssl s_client -connect <ip address>:443
~~~

The obtained certificate is further converted to text using `openssl x509 -text` and saved.

#### Check the Subject CN and SAN fields of the acquired certificate
From the text of the certificates, extract the line with the Subject field and the line with the SAN field.
Then perform a match search using IPv4 address regular expressions for those parts.
If detected, the information of the certificate is saved.

Specifically, we used the grep command on the folder where each certificate is stored as a text file. The commands we used are as follows.

~~~
grep  -r -E '(([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5]$)' <folder name> > <result file> &
~~~

## Result

### Survey of Certificate Policy (CP) and Certification Practice Statement (CPS)  of Major Web Server Certificate Issuer
|Issuer|Regulations|Location|Descriptions|
----|----|----|----
|Let's Encrypt|Yes|CP "3.2.2.5 Authentication for an IP Address"|- Agreed-Upon Change to Website<br>- Email, Fax, SMS, or Postal Mail to IP Address Contact<br>- Reverse Address Lookup<br>- Phone Contact with IP Address Contact<br>- ACME “http-01” method for IP Addresses<br>- ACME “tls-alpn-01” method for IP Addresses|
|Sectigo|Yes|CPS "3.2.2.1.2 IP Address Verification"|- Agreed-Upon Change to Website<br>- Email, Fax, SMS, or Postal Mail to IP Address Contact<br>- Reverse Address Lookup|
|DigiCert|Yes|CPS "3.2.2.1 Verification of IP Address"|- Agreed-Upon Change to Website<br>- Email, Fax, SMS, or Postal Mail to IP Address Contact<br>- Reverse Address Lookup<br>- Phone Contact with IP Address Contact<br>- ACME “http-01” method for IP Addresses<br>- ACME “tls-alpn-01” method for IP Addresses|
|SECOM|No|---||
|UPKI|No|---||


### Survey of the Existence of Web IP Certificates
The survey was conducted multiple times. The information for each is shown below.

#### Number of certificates obtained
|Period|# of hosts with port 443 open|# of certs|
----|----:|----:
Nov. 2020| 1,198,326 | 908,071
Aug. 2021| 1,251,339 | 882.735


#### Number of Web IP certificates
|Period|on Subject|on SAN|on Subject or SAN
----|----:|----:|----:
Nov. 2020 | 33,366 | 27,120 | 44,628
Aug. 2021 | 33,067 | 33,067 | 45,935


#### The IP address included in the certificates
on Nov. 2020
| IP Addr.| # of appearances |
----|----:
192.168.168.168 | 5615
192.168.1.1 | 4362
192.168.0.1 | 2275
172.20.0.1 | 1968
10.0.0.1 | 1919
192.168.1.108 | 1904
10.100.0.1 | 1665
127.0.0.1 | 1481
10.252.252.252 | 644
192.168.1.101 | 422


on Aug. 2021
| IP Addr.| # of appearances |
----|----:
192.168.168.168 | 5148
192.168.1.1 | 4036
172.20.0.1 | 3095
10.100.0.1 | 2380
10.0.0.1 | 2281
192.168.1.108 | 1645
127.0.0.1 | 1569
10.252.252.252 | 621
192.168.0.1 | 613
10.3.240.1 | 442


## Publication
Akira Kanaoka, Yuki Oyama, Masayuki Okada, "***Study of Web Server Certificates Containing IP Addresses As Subjects***", 2022 Symposium on Cryptography and Information Security (SCIS 2022), 2022  (published in Japanese)

## Contact
Akira Kanaoka (Toho University)
akira.kanaoka@is.sci.toho-u.ac.jp

## Acknowledgement
This work was supported by JSPS KAKENHI Grant Number JP19K11972.