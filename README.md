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
