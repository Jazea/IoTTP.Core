IoTTP.Core
======

Internet of things transport protocol (IoTTP) library for edge computing based on MQTT protocol. 

Support platform
-----------
* Windows
* Linux
* MacOS
  
How to use?
-----------
Device (broker, executive command)
```
var serverUri = new Uri("iottp://rabbitserver:1883"); 
var userName = "guest";
var password = "guest";

//Device unique, if empty system automatically generated
var deviceId = new Guid("97c8bc6e-0709-4b2e-82ef-298f42d91dcc");
var broker = new IoTTP.Core.Broker(deviceId);
await broker.StartAsync(serverUri, userName, password);
```

Client (send command)
```
var serverUri = new Uri("iottp://rabbitserver:1883"); 
var userName = "guest";
var password = "guest";

//find all the devices you can control
//var queueApi = $"http://{serverUri.Host}:{15672}/api/queues";
//var devices = await Discover.FindAsync(new Uri(queueApi), userName, password);

//consistent with device side, Or from the devices found.
var deviceId = new Guid("97c8bc6e-0709-4b2e-82ef-298f42d91dcc");
var client = new Client(new Guid(deviceId));
await client.ConnectAsync(serverUri, userName, password);

var commandLine = Console.ReadLine(); //for example: "ping bing.com"
var command = new CommandBuilder().FromLine(commandLine).Build();
var requestMessage = new RequestMessage(command, command.Method);
await client.RequestAsync(requestMessage, (ResponseMessage<string> responseMessage) =>
    {
        //The return contents of the program can be defined on the device side, such as the json string after the object has been serialized, where it can be deserialized.
        var content = responseMessage.Content;
        Console.WriteLine(content); //
    });
```
--------
TODO
-------------
* Device side security verification
* Support for SSL/TSL
* Test performance in different operating system environments
* And much more...
--------
Build Status
------------

In progress

Proposal
------------
We welcome people of insight to develop this project together. Thank you.