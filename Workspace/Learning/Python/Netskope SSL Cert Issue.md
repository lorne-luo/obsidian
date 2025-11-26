
```
[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1129)
```


# 1. install pip-system-certs
```
pip install pip-system-certs --use-feature=truststore
```

# 2. Export **SSL_CERT_FILE**
```
export SSL_CERT_FILE=/etc/ssl/certs/your_cert.pem
export REQUESTS_CA_BUNDLE=/etc/ssl/certs/your_cert.pem
```

# 3. Downgrade `certifi`
```
pip install certifi==2025.1.31

export SSL_CERT_FILE=$(python3 -m certifi)

os.environ['SSL_CERT_FILE'] = certifi.where()

```

# 4. Disable SSL Verify
```
response = requests.get(url, verify=False)
```

