# G8OS Hub Direct Client
Python client to query and send data to a `hub-direct-server`

# Exemple
Request not existing keys
```
remote = client.Client('http://127.0.0.1:5000')

req = remote.exists.exists_post(["aGVsbG8K", "d29ybGQK"])
keys = req.json()
```

Inserting keys content
```
upload = ()
upload += (('files[]', ("aGVsbG8K", "HelloWorld")),)
upload += (('files[]', ("aGV5aGV5JAo=", "HelloWorldMultiData")),)

remote.insert.insert_put(upload)
```
