Service Definition
==================

The Arrowhead Framework has a [service oriented architecture (SOA)](https://en.wikipedia.org/wiki/Service-oriented_architecture).
Yet, it is interesting to ask people what is a service.

We here consider three incarnations of a service: in real life, in memory and in messages or payloads.

## Real services
The English version of service, although correct, is not as rich as the French or Swedish one.
This can be seen in the translation of SOA as *[Architecture orientée services](https://fr.wikipedia.org/wiki/Architecture_orientée_services)* and *[Tjänsteorienterad arkitektur](https://sv.wikipedia.org/wiki/Tjänsteorienterad_arkitektur)* respectively.
In French one asks: "Peux tu me rendre un service?", while in Swedish one asks "Kan du göra mig en tjänst?".
Returning to English, this translates as "Can you do me a favor?".

In real life favors take one of two forms: an information or an actuation.
For example, "Can you please tell me how warm it is outside?" is a request for information.
Another example would be "Can you please give me a cup of coffee?", which is a request for an actuation.
Using a smart phone, one can request an actuation from one's bank via an app to transfer funds from one account to another; this use to be done by a bank teller before the Internet and SOA.

With its service oriented architecture, the Arrowhead Framework's prosumers provide and consume from each other via messages.

## Services in memory
The Arrowhead Framework's prosumers are running programs or applications on a computer in which these services are stored in memory.
The services are modeled as objects with attributes and methods or functions.
For example the intense of temperature type object would have the temperature value, units and time stamp as (3) attributes.
It would also have method such as 'getTemperature()' and 'setTemperature(float temperature)'.

## Exchange of services
When prosumers provide and consume services, they do it by exchanging messages as [XML](https://en.wikipedia.org/wiki/XML) or [JSON](https://en.wikipedia.org/wiki/JSON) files in the same way one's web browser gets an [HTML](https://en.wikipedia.org/wiki/HTML) file.
The information in these files allows the prosumer to fill out or update the attributes of the service object using its methods.
This is similar to web [application programming interfaces (API)](https://en.wikipedia.org/wiki/Web_API).