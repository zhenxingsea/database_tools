```mongod --dbpath "E:\mongodb\data\db" --logpath "E:\mongodb\data\log\MongoDB.log" --install --serviceName "MongoDB"
db.createUser({user:"admin",pwd:"root",roles:[{"role":"userAdmin","db":"admin"},{"role":"root","db":"admin"},{"role":"userAdminAnyDatabase","db":"admin"}]})
sc delete MongoDB
mongod --dbpath "E:\mongodb\data\db" --logpath "E:\mongodb\data\log\MongoDB.log" --auth --install --serviceName "MongoDB"
mongo -u admin -p root localhost:27017/admin
use natural_language
db.createUser({user:"language",pwd:"language",roles:[{"role":"readWrite","db":"natural_language"}]})```
