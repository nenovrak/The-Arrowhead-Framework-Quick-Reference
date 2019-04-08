# The developers guide to arrowhead

## Introduction
Arrowhead is a Service-Oriented Architecture (SOA) created for efficient IoT automation. It helps you with the integration of different devices and protocols in your production, allows you to easily log and monitor important variables, and assures your production and communication operates in a secure manner. Arrowhead Framework allows you to *spend most of your time focusing on achieving your higher-level goals such as workflow optimization, quality assurance, custom production, and more*.

## Table of contents

## The local cloud

## Core services
The Arrowhead framework consists of a set of core systems that exist in each Arrowhead local cloud. We will begin to consider these services from a higher abstraction level and then go through each system in more detail. Starting from the five Ws we get a great view on the core systems:  

* **Who** - *Who can be part of a local cloud and who can use the different services?* This question is answered by the **Authorization** core system.
* **What** - *What services exist and how do I connect to them?* The **Orchestrator** core system allows one to search for services both within the local cloud but also in other connected local clouds. 
* **When** - *How do I schedule important events?* In order to facilitate the periodic scheduling, and to signal important events across services you can use the **EventHandler** core system.
* **Where** - *Where do I connect to other local clouds?* In order to interact with other local clouds in an easy and secure fashion, you should use the **GateKeeper** core service.
* **Why** - *Why did that happen?* The **Logger** core service can help you by providing important information and status about your local cloud that is useful when trying to find problems.

The picture below shows the different core systems.  
**PICTURE OF CORE SYSTEMS HERE...**


##  Authorization
The purpose of the Authorization System is to:  

* Provide AuthorizationControl Service (both intra- and inter-Cloud)
* Provide a TokenGeneration Service for allowing session control within the Local Cloud

This core system holds a database that describes which Application System can consume what Services from which Application Systems (intra-Cloud access rules) and which other Local Clouds are allowed to consume what Services from this Cloud (inter-Cloud authorization rules). The Authorization system is also in charge of the authentication of Systems in the local cloud. Authentication is achieved using X.509 certificates, and the Authorization system stores all X.509 certificate PublicKeys for every System in the Cloud. Authentication between Systems in the local cloud is facilitated by having the systems  passing session tokens between them that they have acquired from the Authorization System. Each system can verify the identity of another party by contacting the Authorization System and ask for the public key of the to-be-authenticated system.

**PICTURE OF TOKEN GENERATION AND PASSING HERE...**

**PICTURE OF INTRA-CLOUD AUTH TABLE HERE...**


### Produced services
The Authorization System offers two Core Services:  

* AuthorizationControl <-- **MAKE LINK TO AUTHIRAZTION SERVICES HERE...**
* TokenGeneration <-- **MAKE LINK TO AUTHIRAZTION SERVICES HERE...**

The purpose of the *TokenGeneration* functionality is to create session control functionality through the Core Sytems. The output is an ArrowheadToken that validates the Service Consumer system when it will try to access the Service from another Application System (Service Provider). This Token shall be primarily generated during the orchestration process and only released to the Service Consumer when all affected Core Systems are notified and agreed to the to-be-established Service connection. 

This System (in line with all core Systems) utilizes the X.509 certificate Common Name naming convention in order to work. This means that the CN is structured as it is described in the generic G4.0 System-of-System Design Document (SoSDD). 
