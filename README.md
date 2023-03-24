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
```python
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
