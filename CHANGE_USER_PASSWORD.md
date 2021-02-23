1. Go to the terminal
2. Run node
3. Write:
```js
const crypto = require('crypto')
crypto.pbkdf2Sync(<SOME-NEW-PASSWORD>, <THE-CONFIGURATION-SALT-(BCF)>, 1000, 64, "sha1").toString("hex")
```
4. Copy the output
5. Connect with the DigitalOcean mongo server
6. Access the desired database with the mongo client
7. Write (assuming the collection is called users):
```js
db.users.update({_id: ObjectId("<USER-ID>")}, {$set: {hash: <NEW-PASSWORD>}})
```
8. Done!
