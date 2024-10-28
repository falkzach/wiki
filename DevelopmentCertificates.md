# Development Certificates

At times it is necessary to run a full ssl development environment, often due to increasingly strict policies and authentication environments. It may be benificial to generate development certificates in a centralized way for them to be referenced in various projects.

## Config files
Create the following config files to generate certificates

Modify values accordingly

`server.csr.cnf`
```bash
[req]
default_bits = 2048
prompt = no
default_md = sha256
distinguished_name = dn

[dn]
C=US
ST=MT
L=Missoula
O=DevelopmentOrganization
OU=Development
emailAddress=email@example.com
CN=127.0.0.1
```

`v3.ext`
```bash
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEcipherment, dataEnciphrment
subjectAltName = @alt_names
[alt_names]
DNS.1 = localhost:3000
DNS.2 = dev.fqdn.com
DNS.3 = dev.fqdn2.com
```

## Certificate Authority (CA)

Generate `RootCA.pem`, `RootCA.key`, and `RootCA.crt`

```bash
openssl req -x509 -nodes -new -sha256 -days 1024 -newkey rsa:2048 -keyout RootCA.key -out RootCA.pem -subj "/C=US/CN=Root-Dev-CA"
openssl x509 -outform pem -in RootCA.pem -out RootCA.crt
```

Generate `localhost.key`, `localhost.csr`, and `localhost.crt`

```bash
openssl req -new -nodes -newkey rsa:2048 -keyout localhost.key -out localhost.csr -config server.csr.cnf
openssl x509 -req -sha256 -days 1024 -in localhost.csr -CA RootCA.pem -CAkey RootCA.key -CAcreateserial -extfile v3.ext -out localhost.crt
```

Generate `localhost.pfx`, set `localhost` as the password

```bash
openssl pkcs12 -export -out localhost.pfx -inkey localhost.key -in localhost.crt
```

## Trust the root certificate

In your browser, naviate to the certificate manager and import the RootCA certificate

## Reference the dev certs

### dotnet 6/7

Add the following to the User appsettings

`appsettings.User.json`
```json
  "kestrel": {
    "Http": {
        "Url": "http://*:5000"
    },
    "Https": {
        "Url": "http://*:5001"
    },
    "Certificate": {
        "Path": "../path/to/certs/localhost.pfx",
        "Password": "localhsot"
    }
  }
```
