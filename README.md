# pi

### Download and install MQTT broker mosquitto from http://mosquitto.org/download/

Start the service

### Use the command line tools to test the service

Open a window to run this:
```
  mosquitto_sub.exe -d -t hello/world
```
Open another window to run this:
```
  mosquitto_pub.exe -d -t hello/world -m "you are so cool"
```


