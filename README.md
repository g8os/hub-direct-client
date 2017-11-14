# Zero-OS Hub Direct Client
Python client to query and send data to a `hub-direct-server`

# Installation
This module is installable via pip:
```
pip install --user -e 'git+https://github.com/zero-os/hub-direct-client#egg=hubdirect'
```

# Authentification
This service allows you to upload data to the backend, with authentification.
You need to have a valid account on itsyou.online. With a `jwt` token, you can use this service very easily.

Without a valid token, you can't access the backend.

Your username doesn't needs to be part of a special organization, but your username will be logged
for any operation you do.

# How to use the client
At first, authentificate yourself with a itsyou.online API key:
```python
clientid = ''
clientsecret = ''
response = requests.post('https://itsyou.online/v1/oauth/access_token?grant_type=client_credentials&client_id=%s&client_secret=%s&response_type=id_token' % (clientid, clientsecret))

if response.status_code != 200:
    raise RuntimeError("Authentification failed")

jwt = response.content.decode('utf-8')
```

Link your jwt to the client:
```python
from zeroos.hubdirect import Client

hubclient = Client('https://direct.hub.gig.tech')
hubclient.set_auth_header('bearer %s' % jwt)
```

Checking for keys existance. The resulting list are keys not found on the backend.

**Warning**: keys must be `binary`, they will be base64 encoded on client-side.
```python
notfound = hubclient.api.exists.exists_post([b"key1", b"key2"]).json()
```

Inserting non existing keys contents (now, you need to base64 yourself keys and content):
```python
upload = ()
upload += (('files[]', (base64.b64encode(b"key1"), base64.b64encode(b"HelloWorld"))),)
upload += (('files[]', (base64.b64encode(b"key2"), base64.b64encode(b"HelloWorldMultiData"))),)

hubclient.api.insert.insert_put(upload)
```

**Warning**: if you try to insert a key already existing, by security, the whole insertion is stopped.
