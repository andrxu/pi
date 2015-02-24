# pi

### Install a MQTT broker

1. Download and install mosquitto from http://mosquitto.org/download/
2. Start the service

### Use the command line tools to test the service

* Open a window to run this:
```
  mosquitto_sub.exe -d -t hello/world
```
* Open another window to run this:
```
  mosquitto_pub.exe -d -t hello/world -m "you are so cool"
```

### Create and run a nodejs client service

* Create a file with following code and save it as "sub.js"

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
* Start the node service:
```
 node sub.js
```

### Create html page show the messages from MQTT broker

```html
<html>
<head>
<script type="text/javascript" src="http://cdn.socket.io/socket.io-1.3.4.js"></script>
<script type="text/javascript" src="http://code.jquery.com/jquery-1.11.2.min.js"></script>
<script type="text/javascript">
    var socket = io.connect("http://localhost:5000");
    socket.on('connect', function () {
        socket.on('mqtt', function (msg) {
            var elmarr = msg.topic.split("/");
            var elm = elmarr[3];
            console.log(msg.topic + ' ' + msg.payload);
            $('#returntemp').html(msg.payload);
        });
        socket.emit('subscribe', { topic: 'hello/world' });
    });
</script>
</head>
<body>
<h1>Real Time Messaging Between pi and Web app</h1>
<table style="width: 500px;">
    <tbody>
        <tr>
            <td colspan="2"><center>Status</center></td>
        </tr>
        <tr>
            <td>Return temp</td>
            <td id="returntemp"></td>
        </tr>
    </tbody>
</table>
</body>
</html>

```
* Save the page as mtqq_sub.html and put it somewhere under your local web site directory
* Load it from browser, for example, http://localhost/mtqq_sub.html
* Use command line tool to send messages 
```
 mosquitto_pub.exe -d -t hello/world -m "you are so cool"
```
* mtqq_sub.html should display the message you just sent


