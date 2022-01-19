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

#### Survey of TCP port 443 openings for all IPv4 addresses

#### https (TLS) access to IP address with port 443 open

#### Check the Subject CN and SAN fields of the acquired certificate.

## Result

### Survey of Certificate Policy (CP) and Certification Practice Statement (CPS)  of Major Web Server Certificate Issuer

### Survey of the Existence of Web IP Certificates

#### Number of certificates obtained

#### Number of Web IP certificates

#### The IP address included in the certificates

## Publication
Akira Kanaoka, Yuki Oyama, Masayuki Okada, "***Study of Web Server Certificates Containing IP Addresses As Subjects***", 2022 Symposium on Cryptography and Information Security (SCIS 2022), 2022  (published in Japanese)

## Contact
Akira Kanaoka (Toho University)
akira.kanaoka@is.sci.toho-u.ac.jp

## Acknowledgement
This work was supported by JSPS KAKENHI Grant Number JP19K11972.