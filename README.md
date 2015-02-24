# pi

### Install a MQTT broker

1. Download and install mosquitto from http://mosquitto.org/download/
2. Start the service

### Use the command line tools to test the service

Open a window to run this:
```
  mosquitto_sub.exe -d -t hello/world
```
Open another window to run this:
```
  mosquitto_pub.exe -d -t hello/world -m "you are so cool"
```

### Create and run a nodejs client service
```js
var sys = require('sys');
var mqtt = require('mqtt');
var client = mqtt.connect('mqtt://localhost');

var io = require('socket.io').listen(5000);

client.subscribe('hello/world');
//client.publish('hello/world', 'Hello mqtt from sub.js');

console.log('io started...');

io.sockets.on('connection', function (socket) {
    socket.on('subscribe', function (data) {
        console.log('Subscribing to ' + data.topic);
        client.subscribe('hello/world');
    });
});


client.on('message', function (topic, message) {
    console.log(message.toString());
    sys.puts(topic + '=' + message);
    io.sockets.emit('mqtt', {
        'topic': String(topic),
        'payload': String(message)
    });

});

var express = require('express')
var app = express()

app.get('/', function (req, res) {
    res.send('Hello World!')
})

var server = app.listen(3000, function () {

    var host = server.address().address
    var port = server.address().port

    console.log('Example app listening at http://%s:%s', host, port)

})

//client.end();

```
node sub.js

### Create html page show the messages from MQTT broker



