IoTTP.Core
======

Internet of things transport protocol (IoTTP) library for edge computing based on MQTT protocol. 

Feature(implemented)
-----------
* Executes a remote command and returns the result dynamically
* Client decides whether to return an execution result as needed
* Remote command execution log (process start and stop state)
* Support multi-session, multi-process concurrent execution, return result session isolation
* Support client specified command execution duration (expiration is automatically cancelled to avoid resource waste)
* Whether the same command is Multiton executed
* Support for communicating with child processes via named pipes (passing execution parameters)

Support platform
-----------
* Windows
* Linux
* MacOS
  
How to use?
-----------
**Device** (broker, executive command)
```
var serverUri = new Uri("iottp://broker.hivemq.com");
var userName = "guest";
var password = "guest";

//Device unique, if empty system automatically generated
var deviceId = new Guid("97c8bc6e-0709-4b2e-82ef-298f42d91dcc");
var broker = new IoTTP.Core.Broker(deviceId);
await broker.StartAsync(serverUri, userName, password);
```

**Client** (send command)
```
var serverUri = new Uri("iottp://broker.hivemq.com");
var userName = "guest";
var password = "guest";

//find all the devices you can control(For now only RabbitMQ)
//var queueApi = $"http://{serverUri.Host}:{15672}/api/queues";
//var devices = await Discover.FindAsync(new Uri(queueApi), userName, password);

//consistent with device side, Or from the devices found.
var deviceId = new Guid("97c8bc6e-0709-4b2e-82ef-298f42d91dcc");
var client = new Client(deviceId);
await client.ConnectAsync(serverUri, userName, password);

var commandLine = Console.ReadLine(); //for example: "ping bing.com"
var command = new CommandBuilder().FromLine(commandLine).Build();
var requestMessage = new RequestMessage(command, command.Method);
await client.RequestAsync(requestMessage, (ResponseMessage<string> responseMessage) =>
    {
        //The return contents of the program can be defined on the device side, such as the json string after the object has been serialized, where it can be deserialized.
        var content = responseMessage.Content;
        Console.WriteLine(content);
    });
```
--------
TODO
-------------
* Device side security verification
* Support for SSL/TSL
* Offline storage, online retransmission
* Test performance in different operating system environments
* And much more...
--------
Build Status
------------

In progress

Proposal
------------
We welcome people of insight to develop this project together, Thanks.