---
title: security
date: 2017-06-13T11:48:45+08:00
lastmod: 2018-07-06T16:52:44+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## How to open a CRL file?
```
openssl crl -inform DER -text -noout -in mycrl.crl
```
## How do I verify that a private key matches a certificate?
```
How do I verify that a private key matches a certificate?
To verify that an RSA private key matches the RSA public key in a certificate you need to i) verify the consistency of the private key and ii) compare the modulus of the public key in the certificate against the modulus of the private key.

To verify the consistency of the RSA private key and to view its modulus:
openssl rsa -modulus -noout -in myserver.key | openssl md5

To view the modulus of the RSA public key in a certificate:
openssl x509 -modulus -noout -in myserver.crt | openssl md5
```
## [How to display content of certificate?](http://support.qacafe.com/knowledge-base/how-do-i-display-the-contents-of-a-ssl-certificate/)
```
openssl x509 -in acs.qacafe.com.pem -text
penssl x509 -in MYCERT.der -inform der -text
```


## Reference
## [PKCS#11 wiki](https://en.wikipedia.org/wiki/PKCS_11)
## [cfssl introdution](https://blog.cloudflare.com/introducing-cfssl/)
## [cfssl on github](https://github.com/cloudflare/cfssl)
## [oscca](http://www.oscca.gov.cn/#)
## [hongwei gitbook](https://hongweigithub.gitbooks.io/adc-book/content/verifone-apisdk/sm.html)
## [Simply put: How does certificate-based authentication work?](http://www.networkworld.com/article/2226498/infrastructure-management/simply-put-how-does-certificate-based-authentication-work.html)


## Investigation for fabric security solution for China
### CA
## Certificate chain for [ftsafe 飞天诚信](https://www.ftsafe.com.cn/): 
  + [StartCom](https://www.startssl.com/AboutUS)
  + [WoSign 沃通](https://www.wosign.com/)

## China CA
    + [CFCA(China Financial Certificate Authority)](https://www.cfca.com.cn/): Signature Algorithm: SHA-256 with RSA Encryption. But also support SM2/SM3. Refer to [certificate chain diagram](http://www.cfca.com.cn/20150811/101230561.html) for detail.
    + CTCA:中国电信CA认证中心
    + [SHECA(Shanghai Electronic Certificate Authority Center)](https://www.sheca.com/Home)

## SM2/3/4 implementations: [gmss in clang](http://gmssl.org/A)
## Encryption machine implementing SM2/3/4: http://www.keyou.cn/product/product-detail.aspx?id=33


### [Yubico](https://www.yubico.com/)
## [OTP VS. U2F: STRONG TO STRONGER ](https://www.yubico.com/2016/02/otp-vs-u2f-strong-to-stronger/)
## [U2F](https://en.wikipedia.org/wiki/Universal_2nd_Factor)

### Reference:
## [SafeNet on AWS](http://www.secdoctor.com/html/hyxx/26050.html)
## [商用密码管理条例](http://www.oscca.gov.cn/News/200512/News_1053.htm)
## [PPT on SM2 plan](https://www.ietf.org/proceedings/87/slides/slides-87-cfrg-2.pdf)
## [SM2](http://www.oscca.gov.cn/News/201012/News_1198.htm)
## [国家密码管理局公告](http://www.oscca.gov.cn/News/201204/News_1228.htm)
## [Internet X.509](https://tools.ietf.org/html/rfc5280#section-4.2.1.3)
## [yubihsm](https://www.yubico.com/products/yubihsm/)
## [Next Consensus Architecture Proposal](https://github.com/hyperledger-archives/fabric/wiki/Next-Consensus-Architecture-Proposal)
## [Secure Hash](https://en.wikipedia.org/wiki/Secure_Hash_Algorithm)
## [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography)
## [Elliptic Curve Digital Signature Algorithm](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)
## [cipher machines](http://ciphermachines.com/)
