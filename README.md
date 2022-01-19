Web IP Certificate Survey
====
日本語版READMEは[こちら](/README-jp.md)

There exists a certificate whose server's IP address is set in CN (Common Name) in the Subject field or SAN (Subject Alternative Name)  field of a Web server certificate instead of FQDN (Fully Qualified Domain Name).
We call such certificates WebIP certificates.

With a WebIP certificate, a client's SSL/TLS connection to a server with an IP address will be verified as a valid communication partner. When a Certificate Authority (CA) issues a WebIP certificate, its rules may not check whether the server or server administrator retains the right to use or own the IP address. As a result, a third party who does not have ownership of the IP address may issue a certificate for that IP address.

Despite these risks, there has been no survey on how many certificates are issued. In this study, we obtained certificates through TLS connections to all IPv4 addresses and investigated the existence of WebIP certificates and the characteristics of IPv4 addresses used.

## Web IP Certificates?
Web IP certificate is...


## Programs

## Data

## Result