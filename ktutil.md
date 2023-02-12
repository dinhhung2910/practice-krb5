# `ktutil`

## Generate keytab file

```
ktutil: add_entry -password -p <principal> -k 1 -e <encryption type>

# write keytab
ktutil: wkt <path/to/keytab>
```

Common encryption type in latest kerberos 5:

* `aes128-cts-hmac-sha1-96`

* `aes256-cts-hmac-sha1-96`

* `aes128-cts-hmac-sha256-128`

* `aes256-cts-hmac-sha384-192`

* `camellia128-cts-cmac`

* `camellia256-cts-cmac`