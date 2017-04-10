## IoT scenario

### Device: Raspberry Pi
The Raspberry Pi is a series of small single-board computers. https://www.raspberrypi.org/

#### API

1. gpiozero: A simple interface to everyday GPIO components used with Raspberry Pi. (https://gpiozero.readthedocs.io/en/stable/index.html)

2. LED control example: https://gpiozero.readthedocs.io/en/stable/index.html

### Device Usage and Workflow

1. LED light on/off button: following example below. And set up circuit as below: 
https://www.raspberrypi.org/learning/python-quick-reaction-game/worksheet/
2. implement [call back function](../master/raspberry_pi_trigger.py#L23-L35) when button for LED light on/off
3. when LED on/off triggered, [a message will be sent](../master/raspberry_pi_trigger.py#L8-L21) to EC2 server (described below)
4. [server process](https://github.com/keypointt/ControlLED#server---flask--nginx) request with LED light status
5. web browser/apps query server for LED light status and [display to end user](https://github.com/keypointt/ControlLED#front-end-hookup)

## Backend Usage

1. ngnix server up
2. flask server up
3. mongod up

## Server - Flask & Nginx

### Install
`sudo pip install Flask`

`sudo pip install flask-pymongo`

### Run Server - Flask
```
cd /path/to/code
export FLASK_DEBUG=1
export FLASK_APP=server.py
flask run --host=0.0.0.0
```

### Flask Server deployment

1. spin up ec2 instance (Amazon Linux)
2. get `.pem` key
3. `ssh -i /Users/xinr/Downloads/sapHackathon.pem root@ec2-54-xx-x-xxx.us-west-2.compute.amazonaws.com` 
4. setup Mongo on centOS: https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/


(`@note`: change .pem file to 600: `chmod 600 /Users/xinr/Downloads/sapHackathon.pem`)

(`@note`: user name is `root`, not `centos`. aws turotial is wrong at http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)

http://flask.pocoo.org/docs/0.12/deploying/#deployment

### Nginx Server deployment
1. `sudo yum install nginx`
2. `sudo /etc/init.d/nginx start`
3. nginx will redirect requests to localhost(127.0.0.1)

Ref: http://vladikk.com/2013/09/12/serving-flask-with-nginx-on-ubuntu/

## Client

### status request
http://localhost:5000/status?udid=d1

### udpate request
http://localhost:5000/update?udid=d1&status=closed

`@note`: I didn't do param checking or exeception handling, here is just a super simple demo

## MongoDB

### PyMongo
`sudo python -m pip install pymongo`

https://api.mongodb.com/python/current/tutorial.html

### MongoDB Server
`sudo mkdir -p /Users/xinr/Downloads/mongodb/`

`mongod --dbpath /Users/xinr/Downloads/mongodb/`

### MongDB Query
```
use local
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

## Front-End Hookup

### Usage

1. click 'Add' to add a green circle
2. Drag circle to map
3. click 'Refresh' 

### Color and Room Status
* Green circle: room not occupied
* Red circle: room is occupied

### Endpoint
http://ec2-34-209-46-58.us-west-2.compute.amazonaws.com:5000/

### return html file
1. create a folder called `templates`
2. and use built-in `render_template`
```
def hello(name=None):
    return render_template('hello.html', name='name')
```

Ref: http://flask.pocoo.org/docs/0.12/quickstart/#rendering-templates

### for css and js files
1. need to have a `static` folder setup (for css/js files)
2. then: `<link rel= "stylesheet" type= "text/css" href= "{{ url_for('static',filename='styles/mainpage.css') }}">`

Ref: http://stackoverflow.com/questions/22259847/application-not-picking-up-css-file-flask-python
