---
layout: post
title: Importing SSL Certificates to a Keystore with Java Keytool
date: '2013-12-20T17:50:00.000+01:00'
author: Peter Keller
tags:
- Cryptography
modified_time: '2013-12-20T17:50:26.184+01:00'
blogger_id: tag:blogger.com,1999:blog-7980432895360710298.post-5945950741133935192
blogger_orig_url: http://peter-on-java.blogspot.com/2013/12/importing-ssl-certificates-to-keystore.html
---

Java Keytool is a key and certificate tool for managing cryptographic keys, X.509 certificate chains, 
and trusted certificates.

## Keytool Functions

- Administration of public/private key pairs and associated certificates.
- Administration of secret keys used in symmetric encryption/decryption (e.g. DES)
- Storing keys and certificates in a keystore

In this post, I focus on the last aspect.

## SSL Basics
 
File types
We distinguish between certificates and keystores:

 - **Certificate**: A digitally signed statement from one entity (person, company, etc.), 
 saying that the public key (and some other information) of some other entity has a 
 particular value. When data is digitally signed, the signature can be verified to check 
 the data integrity and authenticity. Integrity means that the data has not been modified or 
 tampered with, and authenticity means the data indeed comes from whoever claims to have 
 created and signed it.
 - **Keystore**: Archive file (database) for storing many cryptography objects such as 
 certificates as a single file. 

## Certificate encodings and extensions

- `.DER`: Binary DER encoded certificates. Not routinely used by anything in common usage.
- `.PEM`: ASCII (Base64) encoded DER certificates used for different types of 
X.509v3 files which contain data surrounded with `-----BEGIN CERTIFICATE-----` 
and `-----END CERTIFICATE-----`. PEM stands for Privacy-enhanced Electronic Mail.
- `.CRT`: Used for certificates in DER or PEM format. Most common in *nix systems.
- `.CER`: Alternate extension of `.CRT`. Microsoft convention.

## Keystore formats and extensions

 - `.JKS`: Keystore in Java format, e.g. `$JAVA_HOME/jre/lib/security/cacerts`
 - `.P12`, `.PKCS12`, `.PFX`: PKCS12 certificate keystore file format. It is commonly used to 
 bundle a private key with its X.509 certificate or to bundle all the members of a chain of trust.

## Keytool Commands for Storing Keys and Certificates in a Keystore

### Listing all imported certificates:

{% highlight sh %}
keytool -list -keystore keystore.jks -storepass ***
{% endhighlight %}    

### Importing a single certificate to a keystore:

{% highlight sh %}
keytool -importcert \
    -file mycert.pem \
    -destkeystore keystore.jks \
    -deststoretype jks \
    -deststorepass ***
    -alias myalias
{% endhighlight %}    

### Importing a PKCS12 keystore to a JKS keystore

This time we import not only a simple certificate 
but a whole keystore:

{% highlight sh %}
keytool -importkeystore \
    -srckeystore cert-and-key.p12 \
    -srcstoretype pkcs12 \
    -srcstorepass *** \
    -destkeystore keystore.jks \
    -deststoretype jks \
    -deststorepass *** \
{% endhighlight %}    

If the destination keystore does not already exists it will be built. So the importing process 
becomes a format change process. If you do not enter the source or destination store passwords, 
you will be prompted for it. You may skip the type information if you are lazy and trust the 
keytool that it will recognize the correct type for you.

### Importing a JKS keystore to a PKCS12 keystore

The same command as above but vice versa:

{% highlight sh %}
keytool -importkeystore \
    -srckeystore keystore.jks \
    -srcstoretype jks \
    -srcstorepass *** \
    -destkeystore cert-and-key.p12 \
    -deststoretype pkcs12 \
    -deststorepass *** \
{% endhighlight %}    

## Further Sources

- [http://docs.oracle.com/javase/7/docs/technotes/tools/solaris/keytool.html](http://docs.oracle.com/javase/7/docs/technotes/tools/solaris/keytool.html)
- [https://support.ssl.com/index.php?/Knowledgebase/Article/View/19/0/der-vs-crt-vs-cer-vs-pem-certificates-and-how-to-convert-them](https://support.ssl.com/index.php?/Knowledgebase/Article/View/19/0/der-vs-crt-vs-cer-vs-pem-certificates-and-how-to-convert-them)  

