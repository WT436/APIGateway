 https.config
 ```
 [ req ]
default_bits       = 2048
default_md         = sha256
default_keyfile    = key.pem
prompt             = no
encrypt_key        = no

distinguished_name = req_distinguished_name
req_extensions     = v3_req
x509_extensions    = v3_req

[ req_distinguished_name ]
commonName             = "localhost"

[ v3_req ]
subjectAltName      = DNS:localhost
keyUsage            = critical, digitalSignature, keyEncipherment
extendedKeyUsage    = critical, 1.3.6.1.5.5.7.3.1, 1.3.6.1.5.5.7.3.2 
```
 openssl req -config https.config -new -out csr.pem
 openssl x509 -req -days 365 -extfile https.config -extensions v3_req -in csr.pem -signkey key.pem -out https.crt