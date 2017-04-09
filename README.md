
## Server - Flask
```
export FLASK_DEBUG=1
export FLASK_APP=server.py
flask run --host=0.0.0.0
```

### Flask deployment

http://flask.pocoo.org/docs/0.12/deploying/#deployment

## Client

### status request
http://localhost:5000/status?udid=d1

### udpate request
http://localhost:5000/update?udid=d1&status=closed

`@note`: I didn't do param checking or exeception handling, here is just a super simple demo

## MongoDB

### PyMongo
`python -m pip install pymongo`

https://api.mongodb.com/python/current/tutorial.html

### MongoDB Server
`mongod --dbpath //Users/xinr/Downloads/mongodb/`

### MongDB Query
```
db.status.insert({'deviceId':'d1', 'status':'open'})
db.status.insert({'deviceId':'d2', 'status':'occupied'})
```

### Schema
```
> db.status.find().pretty()
{
	"_id" : ObjectId("58e9681d2533a670f43efb17"),
	"deviceId" : "d1",
	"status" : "open"
}
{
	"_id" : ObjectId("58e9683c2533a670f43efb18"),
	"deviceId" : "d2",
	"status" : "occupied"
}
```
