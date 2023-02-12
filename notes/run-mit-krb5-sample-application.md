# How to run MIT krb5 sample application

## Scenario

Setup 2 hosts

* `host/master.kaito.com@KAITO.COM`: KDC server - to run `sserver`

* `host/client.kaito.com@KAITO.COM`: Client host - to run `sclient`

At least 1 user principal is created (I use `kaito@KAITO.COM`)

## Steps

### 1. Create service `sample/master.kaito.com@KAITO.COM`

```
kadmin: addprinc sample/master.kaito.com
<enter service password>
```

### 2. Generate keytab file for service

```
ktutil: add_entry -password -p sample/master.kaito.com@KAITO.COM -k 1 -e aes128-cts-hmac-sha1-96
ktutil: add_entry -password -p sample/master.kaito.com@KAITO.COM -k 1 -e aes256-cts-hmac-sha1-96
ktutil: add_entry -password -p sample/master.kaito.com@KAITO.COM -k 1 -e aes128-cts-hmac-sha256-128
ktutil: add_entry -password -p sample/master.kaito.com@KAITO.COM -k 1 -e aes256-cts-hmac-sha384-192
ktutil: add_entry -password -p sample/master.kaito.com@KAITO.COM -k 1 -e camellia128-cts-cmac
ktutil: add_entry -password -p sample/master.kaito.com@KAITO.COM -k 1 -e camellia256-cts-cmac
ktutil: wkt /etc/sample.keytab (save this file on master.kaito.com)
```

### 3. Start server

```
master.kaito.com> ./sserver/sserver -p 8888 -s sample -S /etc/sample.keytab
```

### 4. Check connect from client

If it is successful, server will reply with your credential:

```
client.kaito.com> ./sclient/sclient master.kaito.com 8888 sample

connected
sendauth succeeded, reply is:
reply len 34, contents:
You are kaito/@KAITO404.COM
```

## Error encountered

### Client: Server not found in Kerberos database while using sendauth

Reason: Principal `sample/master.kaito.com` doesn't exist. Must create one.

```
kadmin: addprinc sample/master.kaito.com
```

### Server: Service key not available

Reason: Using wrong keytab file when start server

### Server: Decrypt intergrity check failed

Reason: This is the result of a client using a service ticket that does not match the server's service keytab

Solution: 

```bash

# Re-initialize ticket

kdestroy -A
kinit kaito
```