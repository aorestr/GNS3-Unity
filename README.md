# GNS3-Unity #
This work was born as a need for my end-of-studies project. Since its purpose is basically integrate a network simulator ([**GNS3**](https://www.gns3.com/)) in a game engine ([**Unity**](https://unity3d.com)), I needed an API between the two parts of the project in order to make things easier. Of course, the API offers much more possibilities and it's up to you find new, more imaginative uses!

**Unity** works with **C#** so the API has been made entirely in this language. The way it is made, how to make it work and what can do will be explained later on.

## Installation ##
// Esto habrá que rellenarlo al final.

Firstly, you will need [Json.NET](https://www.newtonsoft.com/json) in your system. It's an open-source framework headed to help .NET with JSON.

## First steps ##
// Run y stop GNS3

Once the library is imported into your project and you have **GNS3** running, you can create a new instance of the handler class. The constructor needs at least the ID of the project you want to handle. You can also add the server and port your **GNS3** uses as a server. By default *``host`` = "localhost"* and *``port`` = 3080*:

```csharp
GNS3sharp handler = new GNS3sharp("b4a4f44d-0f62-4435-89e0-84c8c7a2b35f", "localhost", 3080);
```

Ok, so now you have plenty of information and tools within this variable. It contains a list with all the nodes contained in your project. You can access this list with the property ``Nodes``

```csharp
foreach(Node n in handler.Nodes){
	Console.WriteLine(
		"host: {0}, port: {1}, name: {2}, component: {3}",
		n.ConsoleHost, n.Port, n.Name, n.GetType().ToString()
	);
}
```

## Nodes ##
Every node is related to a class which will handle it with the functions you expect this device to have. In order to make this work, the name of your node in your **GNS3** project must start by *[NODETYPE]*. For example, if you plan to have a *VPC* called "My_PC", you must make sure the component is called "[VPC]My_PC" in the project. Every type of node has its own name you can find in the static property called ``label``. For instance, **OpenVSwitch** switches has "OVS" as label and **OpenWRT** routers "OPENWRT".

Every type of node has methods that let you interact with it in a very easy way. For example, once again, if we are dealing with a *VPC* node we can set its IP just like:

```csharp
OurVPC.SetIP("192.168.10.11");
```

We also can select the netmask ("255.255.255.0" by default) and the gateway.

If any of the already created methods can't meet your needs you can always use the generic ``Send`` and ``Receive`` methods:

```csharp
OurVPC.Send("show");
string[] messages = OurVPC.Receive();
```

So far, only five nodes have been made: *VPC*, *EthernetSwitch*, *MicroCore*, *OpenWRT* and *OpenvSwitch*. You can create your own classes for the devices you plan to use. Otherwise, you can use the generic *Node* class instead.
