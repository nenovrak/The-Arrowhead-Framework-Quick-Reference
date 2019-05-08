# Certificate generation for Authorisation system, service consumer and provider in a local cloud using a self-signed certificate authority

This document helps to create X.509 certificates with Elliptic-curve cryptography keys using the curve *secp256r1*. In this document a self-signed root CA certificate is generated, which is further used to sign the certificates of a authorisation server, provider and consumer systems. 

**Requirements

OpenSSL and Java Keytool are needed for certificate generation. Make sure they are added to the PATH environment variable.

Copy the openssl.cnf file provided with this file to the main folder. 

Create the below folder structure before proceeding with creating certificates.

Main

- root
  - private
  	 certs	
- server
  - private
  - certs
  - csr
- SP1
  - private
  - certs
  - csr
- SC1
  - private
  - certs
  - ​csr

## Root CA certificate generation

### Generating root CA private key

```
openssl ecparam -name secp256r1 -genkey | openssl ec -aes-256-cbc -out root/private/root_private_key.pem
```

**Enter PEM pass phrase:** password

### Generating self signed root certificate
```
openssl req -config openssl.cnf -key root/private/root_private_key.pem -new -extensions ext_root -out root/certs/root_cert.pem -x509 -days 7300
```

**Enter pass phrase for root/private/root_private_key.pem:** password
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.

**Country Name (2 letter code) []:** SE

**State or Province Name []:** LLA

**Locality Name []:** LLA

**Organization Name []:** NGAC_Auth_LC

**Organizational Unit Name []:** Root CA

**Common Name []:** Root_CA

**Email Address []:** a@b.com


Generating server certificate signed by Root CA
-------

### Generating server private key

```
openssl ecparam -name secp256r1 -genkey | openssl ec -aes-256-cbc -out server/private/server_private_key.pem
```

### Generating certificate signing request

````
openssl req -config openssl.cnf -new -key server/private/server_private_key.pem -out server/csr/server_csr.pem
````

**Enter pass phrase for server/private/server_private_key.pem:** password
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,

If you enter '.', the field will be left blank.

**Country Name (2 letter code) []:** SE

**State or Province Name []:** LLA

**Locality Name []:** LLA

**Organization Name []:** NGAC_Auth_LC

**Organizational Unit Name []:** NGAC_Auth_Sys

**Common Name []:** NGAC_Auth_Sys

**Email Address []:** a@b.com

### Signing server certificate with Root CA private key

```
openssl x509 -req -extfile openssl.cnf -extensions ext_server -in server/csr/server_csr.pem -CA root/certs/root_cert.pem -CAkey root/private/root_private_key.pem -CAcreateserial -out server/certs/server_cert.pem -days 3650 -sha256
```

### Bundling server private key and certificate in a .p12 file

```
openssl pkcs12 -export -in server/certs/server_cert.pem -inkey server/private/server_private_key.pem -chain -CAfile root/certs/root_cert.pem -name "server" -out server.p12
```

### Importing server.p12 to server keystore

```
keytool -importkeystore -deststorepass password -destkeystore server/certs/server_keystore.jks -srckeystore server.p12 -srcstoretype PKCS12
```

### Adding root certificate to server trust store

```
keytool -import -file root/certs/root_cert.pem -alias root -keystore server/certs/server_truststore.jks -storepass password -noprompt
```

### Adding provider certificate to server trust store

**This command should be executed after generating the providercertificate

```
keytool -import -file SP1/certs/SP1_cert.pem -alias SP1 -keystore server/certs/server_truststore.jks -storepass password -noprompt
```

Generating SP1 certificate signed by Root CA
-----------

### Generating SP1 private key

```
openssl ecparam -name secp256r1 -genkey | openssl ec -aes-256-cbc -out producer/private/producer_private_key.pem
```

### Generating certificate signing request

```
openssl req -config openssl.cnf -new -key producer/private/producer_private_key.pem -out producer/csr/producer_csr.pem
```

### Signing SP1 certificate with Root CA private key

```
openssl x509 -req -extfile openssl.cnf -extensions ext_server -in SP1/csr/SP1_csr.pem -CA root/certs/root_cert.pem -CAkey root/private/root_private_key.pem -CAcreateserial -out SP1/certs/SP1_cert.pem -days 3650 -sha256
```

### Bundling  SP1 private key and certificate in a .p12 file

```
openssl pkcs12 -export -in producer/certs/producer_cert.pem -inkey producer/private/producer_private_key.pem -chain -CAfile root/certs/root_cert.pem -name "producer" -out producer.p12
```

### Importing SP1.p12 to SP1 keystore

```
keytool -importkeystore -deststorepass password -destkeystore producer/certs/producer_keystore.jks -srckeystore producer.p12 -srcstoretype PKCS12
```

### Adding root certificate to SP1 trust store

```
keytool -import -file root/certs/root_cert.pem -alias root -keystore producer/certs/producer_truststore.jks -storepass password -noprompt
```

### Adding server certificate to SP1 trust store

```
keytool -import -file server/certs/server_cert.pem -alias server -keystore producer/certs/producer_truststore.jks -storepass password -noprompt
```

## Generating SC1 certificate signed by Root CA

### Generating SC1 private key

```
openssl ecparam -name secp256r1 -genkey | openssl ec -aes-256-cbc -out producer/private/producer_private_key.pem
```

### Generating certificate signing request

```
openssl req -config openssl.cnf -new -key SP1/private/SP1_private_key.pem -out SP1/csr/SP1_csr.pem
```

### Signing SC1 certificate with Root CA private key

```
openssl x509 -req -extfile openssl.cnf -extensions ext_client -in Producer/csr/producer_csr.pem -CA root/certs/root_cert.pem -CAkey root/private/root_private_key.pem -CAcreateserial -out producer/certs/producer_cert.pem -days 3650 -sha256
```

### Bundling  SC1 private key and certificate in a .p12 file

```
openssl pkcs12 -export -in producer/certs/producer_cert.pem -inkey producer/private/producer_private_key.pem -chain -CAfile root/certs/root_cert.pem -name "producer" -out producer.p12
```

### Importing SC1.p12 to SC1 keystore

```
keytool -importkeystore -deststorepass password -destkeystore producer/certs/producer_keystore.jks -srckeystore producer.p12 -srcstoretype PKCS12
```

### Adding root certificate to client trust store

```
keytool -import -file root/certs/root_cert.pem -alias root -keystore producer/certs/producer_truststore.jks -storepass password -noprompt
```

## Other useful commands

### view contents of a jks file

```
keytool -list -v -keystore keystore.jks
```

### view contents of pem file

```
keytool -printcert -file certificate.pem
```

### delete a certificate from jks

```
keytool -delete -alias mydomain -keystore keystore.jks
```

