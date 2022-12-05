# SSH / openssh

# Introduction - learn some words

It's easy to get lost in the jungle of certificates, certificate signing requests, private-keys, public-keys, pem's, crt's and keys. So first we're going to learn some basics about ssh. This explanations are meant for Linux-Systems. Let's learn some common files related to ssh issues. Usually, you have a folder ~/.ssh in your home directory (the Tilde "~" before the .ssh directory is a shell replacement for your home directory). Here you can find files like:

### authorized_keys
contains public-keys from others. Compare the format of the content inside authorized_keys and compare it with the format of your id_rsa.pub key

### id_rsa and id_rsa.pub
after creating a keypair (ssh-keygen) you get a private-key (which is by default stored in ~/.ssh/id_rsa) and a public-key (which lives in ~/.ssh/id_rsa.pub). Your private-key is for you alone, your public-key goes public. Private keys can have no extension but also can have a .pem extension. There are also .ppk private-keys, these are created from the windows-ssh tool putty. A private key looks somethoing like:
```
-----BEGIN RSA PRIVATE KEY-----
MgIEpAIBAAKtAQEArypWA0S/NWUgya212ytPfHkA20jko/M4+CV+3kHbGPYCS1g/
XKVudbfvkmAnHSmfMItb61pGcuztRX7cDu1mrVHohH73ue3IQ88hmtbAaQQTYYwR
...
h4HtH3nA9sN38brZI5/vd+o3ty96nMM8O+PBI0qxUbKzxxhNKrEMeg==
-----END RSA PRIVATE KEY-----
```

whereas a public key looks like:

```
ssh-rsa AAAAB3NzaC1yc .... 1cGWoe4+R7ZohrNJp username@yourcomputer
```

### known_hosts
here are all servers stored that had already an established connection before. Whenever you accept something like "add these computer to the trusted ones", they'll write their public key into the known_hosts file.

### config
with the file ~/.ssh/config you have a very handy config place to make ssh logins much easier. Check "man ssh_config" for further information. If the config file does not exist you simply can create it.



## Dealing with Certificate signing requests
### Create a certificate signing request (csr)

In order to obtain a valid certificate from an authorized issuer, you need to create a certificate signing request.
The following command creates a private-key and a csr to it:

```
openssl req -nodes -new -newkey rsa:2048 -sha256 -out csr.pem
```




### Check if private-key matches with the csr and the generated certificate

This commandos deliver strings. If the deliver all the same string they belong together.
```
openssl pkey -in private-key.key -pubout -outform pem | sha256sum 
openssl x509 -in certificate.crt -pubkey -noout -outform pem | sha256sum 
openssl req -in CSR.csr -pubkey -noout -outform pem | sha256sum
```



### create a private key, a certificate signature request and a self signed certificate (windows)

create the private-key, we name it server.key
```
openssl.exe genrsa -des3 -out server.key 1024
openssl.exe rsa -out server.key 1024
```

create .pem file

```
openssl rsa -in server.key -out server.pem
```

create server.csr (certificate signature request)

```
openssl req -config c:\openssl.cnf -new -key server.key -out server.csr
```

create unsigned certificate for 1 year

```
openssl x509 -req -days 30 -in server.csr -signkey server.key -out server.crt
```


### check if private-key fits to a certificate (windows)

you need to compare the hash-value of the certificate against the hash-value of the private-key get the certificates hash-value
```
openssl x509 -noout -modulus -in server.crt | openssl md5
```

get the private-keys hash-value

```
openssl rsa -noout -modulus -in myserver.key | openssl md5
```



## Other usefull commands
### Push your public-key to a server you want to communicate with:
```
ssh-copy-id username@host
```

show the random generated image of an ssh key
```
ssh-keygen -lv -f sgb-id_rsa
```

### check details for a https certified website from the shell:
```
openssl s_client -showcerts -servername gnupg.org -connect gnupg.org:443
```

### remove passphrase from private key
```
opessl rsa -in server.key -out server.key
```


