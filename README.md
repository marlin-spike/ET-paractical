# ET-paractical


download  CA certificate [here](https://github.com/marlin-spike/ET-paractical/raw/main/my_company_ca.cer).


```python
# This is a Python code block created by marlinspike
import datetime
from cryptography import x509
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import rsa
from cryptography.x509.oid import NameOID
from cryptography.hazmat.primitives import serialization

# Generate a private key
private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048
)

# Create a CA certificate
issuer = subject = x509.Name([
    x509.NameAttribute(NameOID.COUNTRY_NAME, u"US"),
    x509.NameAttribute(NameOID.STATE_OR_PROVINCE_NAME, u"California"),
    x509.NameAttribute(NameOID.LOCALITY_NAME, u"San Francisco"),
    x509.NameAttribute(NameOID.ORGANIZATION_NAME, u"My Company"),
    x509.NameAttribute(NameOID.COMMON_NAME, u"My Company CA"),
])
cert = x509.CertificateBuilder().subject_name(
    subject
).issuer_name(
    issuer
).public_key(
    private_key.public_key()
).serial_number(
    x509.random_serial_number()
).not_valid_before(
    datetime.datetime.utcnow()
).not_valid_after(
    datetime.datetime.utcnow() + datetime.timedelta(days=365)
).add_extension(
    x509.BasicConstraints(ca=True, path_length=None), critical=True
).sign(private_key, hashes.SHA256())

# Write the CA certificate to a CER file
with open("my_company_ca.cer", "wb") as f:
    f.write(cert.public_bytes(serialization.Encoding.DER))

# Print the CA certificate
print(cert.public_bytes(serialization.Encoding.PEM).decode('utf-8'))
```
output::
```bash
-----BEGIN CERTIFICATE-----
MIIDbzCCAlegAwIBAgIUS8qdoegUib1WIMGdH31ZJ+o8F3AwDQYJKoZIhvcNAQEL
BQAwZzELMAkGA1UEBhMCVVMxEzARBgNVBAgMCkNhbGlmb3JuaWExFjAUBgNVBAcM
DVNhbiBGcmFuY2lzY28xEzARBgNVBAoMCk15IENvbXBhbnkxFjAUBgNVBAMMDU15
IENvbXBhbnkgQ0EwHhcNMjMwMzI0MjAyMjU3WhcNMjQwMzIzMjAyMjU3WjBnMQsw
CQYDVQQGEwJVUzETMBEGA1UECAwKQ2FsaWZvcm5pYTEWMBQGA1UEBwwNU2FuIEZy
YW5jaXNjbzETMBEGA1UECgwKTXkgQ29tcGFueTEWMBQGA1UEAwwNTXkgQ29tcGFu
eSBDQTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOpCEawyvkiEG2LS
kjLbxKi/oX0/DJc1pO3tyQgIUA2kWzJ4ni7tCj7Hud3dRR3HfGGm1ZaKGgiDNT/C
BeBxR7H96/lvIqh8uucWETJLxqV7ctjr6iN2b8uzmDXGk/DwFjMfkJpzoghGbzjh
Vurq7e4F6ZSssUQBXBuMXnYTwlD4FdAN82C9a9IPyNrvlyfzHFQzELD81FrnCqfs
9jXV0Rw4GWaWgAIYYK6O/60kD8h5WBNskGs/w8EEFEvgL4YaZ81tYODewKANoSn4
gbUYcIKEFYO9GEDuMC5jYfKFaHbKrgasB1YmE5pIA7Ag/kCSN3E6r33Gno9rueh8
LvCc1VMCAwEAAaMTMBEwDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG9w0BAQsFAAOC
AQEAMLtGSZfonXcLtbFkYkb8Rma4hspqMWQTvs4+RM0Bl41BRWJkwIP91h0Hsl8p
/HEGi+lp5FApSNHIawnqkurh7JEMh8xouhHMfw6frh51TN+arNcdE96jYqPwKxDk
lGkZqB5tC6usgtEhMRdp+RLU9SSex5cX+atHD5+y0jxgRc+nE1yM1tI8Hgu/UYSR
Lv4M0VifH0ImRnCbUpaeGTOFV6N+Q088bF99p1QqZjNRzJnEJCpqaaWxPOGOQt6x
MxiRmo5nW+TKmVUTJE+ABAQhavadQ4YNxJRN3V0dFkEzhz7B5jxQvIoSLY3xU+Gs
2aFk2+ZmDwQ0SHjH6ikytgdEWw==
-----END CERTIFICATE-----
```
**it will also create the *my_company_ca.cer* Certificate file**

[for more](https://ospkibook.sourceforge.net/docs/OSPKI-2.4.7/OSPKI-html/sample-priv-key.htm).


Sample CA Certificate in TXT formate::
```yaml
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 0 (0x0)
        Signature Algorithm: md5WithRSAEncryption
        Issuer: C=GB, ST=Surrey, O=Best CA Ltd, 
          OU=Class 1 Public Primary Certification Authority, 
          CN=Best CA Ltd
        Validity
          Not Before: Feb  5 19:50:16 2000 GMT
          Not After : Feb  4 19:50:16 2001 GMT
        Subject: C=GB, ST=Surrey, O=Best CA Ltd, 
          OU=Class 1 Public Primary Certification Authority, 
          CN=Best CA Ltd
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
            RSA Public Key: (2048 bit)
              Modulus (2048 bit):
               00:dd:3c:f6:9a:be:d2:66:20:0c:7d:0c:ae:bc:18:
               cc:f4:e8:89:8d:16:b3:5c:16:75:06:33:f9:08:4f:
               d6:9b:f4:6b:e7:4d:0f:44:af:8b:87:dc:79:78:93:
               e8:e4:20:19:df:f0:0d:04:4d:2c:4c:ad:19:b0:31:
               8c:6a:4d:a6:d6:0e:e8:ae:e2:37:75:8d:d5:1e:a2:
               31:15:3c:f4:4d:ad:5d:f8:d0:23:c2:72:de:e2:73:
               9b:ef:f7:84:25:b0:cf:92:4d:39:4a:18:41:ac:91:
               81:28:ac:5b:f2:7d:74:e2:8f:f9:a7:c1:c0:b1:93:
               dd:cd:b1:4c:23:23:63:27:30:4c:da:8e:72:e4:0d:
               77:c2:22:e2:b4:43:bb:9d:ca:36:59:fc:98:91:0c:
               da:c4:2c:34:03:0c:e5:91:51:e2:23:20:ae:68:5e:
               30:8f:9e:f5:a5:2c:e4:bf:ab:2f:fb:82:03:31:b4:
               ff:5e:90:a8:f0:be:b0:4d:aa:f3:af:2c:27:42:c8:
               7e:7a:d2:c3:e8:5b:53:8d:86:db:ae:f6:7c:45:03:
               35:b6:52:9d:a0:c1:e0:da:ac:6b:68:05:7e:f8:73:
               41:62:63:56:b3:47:6e:11:d8:d4:6c:92:be:65:aa:
               f2:a5:72:3d:4e:d9:d2:e2:8d:42:92:3e:cf:39:f9:
               63:89
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier: 
                3C:BA:B3:02:44:B6:18:30:75:0A:53:90:24:22:\
                  9F:4D:24:72:70:E5
            X509v3 Authority Key Identifier: 
                keyid:3C:BA:B3:02:44:B6:18:30:75:0A:53:90:\
                  24:22:9F:4D:24:72:70:E5
                DirName:/C=GB/ST=Some-State/O=Best CA Ltd/\
                  OU=Class 1 Public Primary Certification 
                  Authority/CN=Best CA Ltd
                serial:00

            X509v3 Basic Constraints: 
                CA:TRUE
    Signature Algorithm: md5WithRSAEncryption
        b5:b9:80:5c:b1:29:dc:c0:03:db:28:c8:a3:08:30:ac:41:ea:
        fb:ef:60:b6:b9:ca:57:c5:05:04:fc:2d:29:59:69:ba:80:39:
        30:77:90:f4:0d:23:03:25:1a:95:ff:07:a8:67:8c:02:e8:1e:
        f7:7f:96:06:3e:7e:90:99:b2:e1:19:81:da:5c:97:92:0f:a2:
        ab:5d:ca:0e:c0:b7:52:68:69:89:62:c9:4b:29:90:77:64:80:
        c4:a7:4c:18:4c:68:60:b5:e6:fa:24:58:93:b6:72:ef:5c:9b:
        a0:3a:c7:f6:c5:da:d8:7c:f0:a2:20:1e:e0:04:c0:15:ec:6c:
        dd:73:85:6c:a5:2e:a5:8e:b0:21:6e:28:9a:c1:d0:62:42:54:
        26:b0:17:85:cf:d2:64:17:89:c3:99:94:cf:0d:bd:e5:f0:1a:
        06:37:ea:8c:6b:9e:98:22:df:2e:9d:ad:a0:63:89:76:3b:ff:
        e8:9f:cf:2b:e4:85:89:96:6d:4b:d2:80:3c:7b:87:d1:db:2a:
        c1:1d:71:7a:d1:fe:36:59:a7:6c:19:e1:4a:93:23:6b:c0:68:
        bf:ee:f4:0c:7d:77:46:b1:1a:d7:34:64:46:9d:7f:af:58:36:
        77:ff:35:88:d2:3a:03:b4:29:0d:9e:a1:29:56:78:60:fe:00:
        15:98:7a:17
        ```
        
