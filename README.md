
[Server]
export FLASK_DEBUG=1
export FLASK_APP=server.py
flask run --host=0.0.0.0


[PyMongo]
python -m pip install pymongo
https://api.mongodb.com/python/current/tutorial.html



[MongoDB Server]
mongod --dbpath //Users/xinr/Downloads/mongodb/



[MongDB Query]
db.status.insert({'deviceId':'d1', 'status':'open'})
db.status.insert({'deviceId':'d2', 'status':'occupied'})



[Schema]
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