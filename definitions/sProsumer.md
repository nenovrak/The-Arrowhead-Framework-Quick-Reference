Service Prosumer
====

A service prosumer is an Arrowhead Framework compliant [RAMI 4.0 administrative shell](/figs/shells.jpg) over an asset.

An asset can be:
- a sensor, e.g., a temperature sensor, a pressure sensor, or a complete weather station.
- an actuator, e.g., a pump, a valve.
- a complex combination of sensors and actuators, e.g., a robot manipulator.
- a database, e.g., a service registry or a data manager.
- a human machine interface, e.g., an app on a smart phone.
- another IoT Framework, e.g., [Fiware](https://www.fiware.org).
- an [AI](https://en.wikipedia.org/wiki/Artificial_intelligence) engine.
- an analytics engine.

The service prosumer is a software program and is the basic building block of the Arrowhead Framework.
It has been traditionally been called a [*system*](system.md), which has been confusing to some new comers.
As a basic building block, one can recognize the service prosumer in the Service Registry, Orchestration, Authorization, along with all the other actors in a local cloud.
The name service prosumer comes from the fact that the software program produces and consumes services over Internet protocols from other prosumers in the cloud.
The cohesive aspects of the service prosumers makes it easy to build larger systems.

Having a basic understanding of the internals of a service prosumer helps grasp its cohesive nature.
One can divide a service prosumer in seven slices [^slices] that interact withe each other:
1. The server-client slice: the service prosumer as a server that responds to service requests from other prosumers and makes its own requests from its client component to other service prosumers.
2. The cryptography slice: the communication between services prosumers is encrypted to ensure cybersecurity. The payloads exchanged via the server-client side are encrypted and decrypted here.
3. The packaging slice: the [service](service.md) objects in memory need to be packed and unpacked into  a JSON or XML payload.
4. The authorization slice: for each service exchange between two specific prosumers, an authorization token needs to be available. Since the token is packaged and encrypted, it can only be checked here.
5. The service slice: this is the slice where the [services](service.md) objects are found with their attributes and functions.
6. The asset interface: this is where the service objects interact with the asset.
7. The scheduler: this is the (main) slice that starts up the registry of services, keeps track scheduled events, such as reading a temperature sensor every 0.1 second, or renewing authorization tokens when they are about to expire. In other words, the prosumer responds to service requests made to the server of slice 1 or to scheduled events of slice 7.

A service prosumer inherits its structure from the Prosumer class and is further composed with specific services and asset interface. 
Through inheritance, it gets service objects such as safety, (re)register, (re)orchestrate, shutdown.
For each asset, new specific service objects class have to be derived, which polymorphism allow smooth operation.

[^slices]: Slices are vertical layers since there interactions are described in UML Sequence Diagrams.