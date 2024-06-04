---
title: 'Desirable Techniques: Understanding Modern Messaging with ActiveMQ and ActiveMQ Artemis'
date: '2024-05-28'
image: 'https://raw.githubusercontent.com/arinatechnologies/blogs/main/images/HSM%203.webp'
tags: ['Messaging Techniques' , 'ActiveMQ' , 'ActiveMQ Artemis' , 'Modern Messaging']
---

Welcome to our deep dive into the world of Apache ActiveMQ and its newer version, Artemis. In today's digital era dominated by real-time processing and microservices architectures, the importance of efficient message brokering cannot be overstated. It forms the backbone of communication between different parts of an application, ensuring seamless integration and robust handling of tasks across complex distributed systems.

### What is Apache ActiveMQ?

Apache ActiveMQ is one of the oldest and most trusted open-source message brokers, designed to facilitate high-performance and reliable messaging across multiple protocols. Written in Java, it supports a diverse range of client APIs and offers features like message persistence, delivery guarantees, and advanced routing options. ActiveMQ is commonly used in enterprise environments where robustness and scalability are critical.

### Introduction to Artemis

Artemis, or ActiveMQ Artemis, is the modern successor to ActiveMQ Classic, aiming to address its architectural limitations and enhance scalability and modern feature support. Originating from the HornetQ codebase, Artemis brings improvements such as better performance, simplified clustering, and advanced features like data replication and backup servers.

### Key Features and Comparison

Both ActiveMQ and Artemis offer robust messaging solutions, but their approaches and capabilities differ in several ways:

- **Performance and Storage:** ActiveMQ uses KahaDB for message persistence, which involves a journal and index for message storage. Artemis utilizes an append-only journal that doesn't require a separate index, leading to improved performance.
- **Protocol Support:** ActiveMQ supports various protocols like MQTT, STOMP, and OpenWire. Artemis enhances this by offering a more extensive range of supported protocols and easier configuration, making it adaptable to more diverse environments.
- **Clustering and High Availability:** While ActiveMQ provides basic clustering capabilities, Artemis offers more advanced clustering features with easier setup and automatic failover, making it a better choice for environments where high availability is crucial.
- **Management and Configuration:** Artemis simplifies management through an updated configuration structure and provides out-of-the-box support for features like WebSockets, which are not as straightforward in ActiveMQ.

<table className="ActiveMQ">
                                         <tbody>
                                         <tr>
                                         <th>Criteria</th>
                                         <th>ActiveMQ Classic</th>
                                         <th>Artemis</th>
                                         </tr>
                                         <tr>
                                         <td>IO connectivity layer</td>
                                         <td>Uses tcp (synchronous one) and nio (non-blocking one)</td>
                                         <td>Implemented using Netty, which is a nio framework. Thus need to choose between different implementations as the non-blocking one is used by default. One acceptor can handle multiple messaging protocols on the same port. Web Sockets are supported out of the box. So, there's no need for the separate ws transport like in ActiveMQ, the tcp (Netty) acceptor in Artemis will detect Web Socket clients and handle them accordingly.</td>
                                         </tr>
                                         <td>Message store</td>
                                         <td>Uses KahaDB that consists of a message journal for fast sequential storing of messages (and other command packets) and an index for retrieving messages when needed.</td>
                                         <td>Own message store, which consists only of the append-only message journal and doesn’t require message index.</td>
                                         </tr>
                                         <tr>
                                         <td>Paging (process that happens when broker can't hold all incoming messages in its memory)</td>
                                         <td>Uses cursors, which are basically a cache of messages ready to be dispatched to the consumer. Cursors try to keep all incoming messages in cache. When we run out of the available memory, messages are added to the store, but the caching stops. When the space become available again, the broker will fill the cache again by pulling messages from the store in batches. Because of this, we need to read the journal from time to time during a broker runtime. In order to do that, we need to maintain a journal index, so that messages' position can be tracked inside the journal.</td>
                                         <td>The whole message journal is kept in memory and messages are dispatched directly from it. When we run out of memory, messages are paged on the producer side (before they hit the broker). They are stored in sequential page files in the same order as they arrived. Once the memory is freed, messages are moved from these page files into the journal. With paging working like this, messages are read from the file journal only when the broker starts up, in order to recreate this in-memory version of the journal. In this case, the journal is only read sequentially, meaning that there's no need to keep an index of messages in the journal.</td>
                                         </tr>
                                         <tr>
                                         <td>Message addressing and routing</td>
                                         <td>All other supported protocols, like MQTT and AMQP are translated internally into OpenWire.</td>
                                         <td>Anycast routing is used implement point-to-point semantics, where there'll be only one queue for the address and all consumers will subscribe to it and this addressing and routing scheme is used across all protocols.</td>
                                         </tr>
                                         <tr>
                                         <td>Broker Instance (keeping installation and configuration of the broker separate makes it easier to upgrade and maintain the broker in the future)</td>
                                         <td>Optional step</td>
                                         <td>Need to explicitly create a broker instance</td>
                                         </tr>
                                         <tr>
                                         <td>Configuration file to configure most of the aspects of the broker, like connector ports, destination names, security policies, etc</td>
                                         <td>conf/activemq.xml</td>
                                         <td>etc/broker.xml</td>
                                         </tr>
                                         <tr>
                                         <td>Configure environment variables such as JVM args related to SSL context, debugging, etc</td>
                                         <td>bin/env</td>
                                         <td>etc/artemis.profile</td>
                                         </tr>
                                         <tr>
                                         <td>Logging configuration</td>
                                         <td>etc/logging.properties</td>
                                         <td>etc/logging.properties</td>
                                         </tr>
                                         <tr>
                                         <td>Start the broker in the foreground</td>
                                         <td>$ bin/activemq console</td>
                                         <td>$ bin/artemis run</td>
                                         </tr>
                                         <tr>
                                         <td>Broker as a service</td>
                                         <td>$ bin/activemq start</td>
                                         <td>$ bin/artemis-service start</td>
                                         </tr>
                                         <tr>
                                         <td>Connectors</td>    
                                         <td>OpenWire,
NIO,
AMQP,
STOMP,
VM (OpenWire only),
HTTP (OpenWire-based),
MQTT,
WebSocket (STOMP and MQTT)</td>        
                                          <td>protocols=OpenWire (version 10+),
-,
protocols=AMQP,
protocols=STOMP,
InVM (all protocols, peer to tcp),
-,
protocols=MQTT,
handled by tcp (all protocols)  </td>   
                                           </tr>
                                           <tr>
                                           <td>Destinations</td>
                                           <td>Destinations are 0re-defined in the <destinations> section of the conf/activemq.xml 
                                           configuration file.</td>      
                                           <td>Addresses are defined in <addresses> section of the etc/broker.xml configuration file.</td>
                                           </tr>
                                           <tr>
                                           <td>JMS  Support</td>
                                           <td>JMS 1.1 Supported</td>
                                           <td>JMS 2.0 Supported</td>
                                           </tr>
                                           <tr>
                                           <td>Creation of Durable Subscribers</td>
                                           <td>Creates a durable subscriber per destination. As demand migrates across a network, duplicate durable subs get created on each node in the network but they do not migrate. The end result can result in duplicate message storage and ultimately duplicate delivery, which is not good.</td>
                                           <td>Because a durable subscriber is modeled as a queue, this problem does not arise.</td>
                                           </tr>
                                           <tr>
                                           <td>Authentication</td>
                                           <td>Called Groups. JAAS configuration  through the appropriate broker plugin in conf/activemq.xml</td>
                                           <td>Called Roles.</td>
                                           </tr>
                                           <tr>
                                           <td>Authorization</td>
                                           <td>Specified in conf/activemq.xml</td>
                                           <td>Specified in etc/broker.xml</td>
                                           </tr>
                                           <tr>
                                           <td>Policies</td>
                                           <td>Defined on destination names. To send messages need to define write policy in respective 
                                           brokers</td>
                                           <td>applied to core queues and are more fine grained. To send messages need to define send 
                                           policy in respective brokers</td>
                                           </tr>
                                           <tr>
                                           <td>Project Status</td>
                                           <td>Mature, widely adopted</td>
                                           <td>Active, successor to Classic</td>
                                           </tr>
                                           <tr>
                                           <td>Performance</td>
                                           <td>Good, with some limitations</td>
                                           <td>Improved, high performance</td>
                                           </tr>
                                           <tr>
                                           <td>Persistence Mechanism</td>
                                           <td>Uses KahaDB or JDBC</td>
                                           <td>Uses a fast journal for message persistence</td>
                                           </tr>
                                           <tr>
                                           <td>Broker Architecture</td>
                                           <td>Traditional broker architecture, which can become a bottleneck in high-throughput scenarios</td>
                                           <td>Designed as an asynchronous broker that can handle high-throughput scenarios more efficiently</td>
                                           </tr>
                                           <tr>
                                           <td>Broker Architecture</td>
                                           <td>Traditional broker architecture, which can become a bottleneck in high-throughput scenarios</td>
                                           <td>Designed as an asynchronous broker that can handle high-throughput scenarios more efficiently</td>
                                           </tr>
                                           <tr>
                                           <td>High Availability</td>
                                           <td>Master-slave configuration for high availability</td>
                                           <td>Built-in support for automatic failover and shared-nothing live-backup configurations</td>
                                           </tr>
                                           <tr>
                                           <td>Clustering</td>
                                           <td>Basic clustering support with network of brokers</td>
                                           <td>Advanced clustering with automatic client failover</td>
                                           </tr>
                                           <tr>
                                           <td>Management and Monitoring</td>
                                           <td>JMX based management which can be less intuitive and more complex</td>
                                           <td>Improved JMX management, web console and better support for management over the messaging protocol</td>
                                           </tr>
                                             <tr>
                                          <td>Message Filtering</td>
                                           <td>Basic selector-based filtering</td>
                                           <td>Advanced filtering capabilities including custom plugin support</td>
                                           </tr>
                                           <tr>
                                           <td>Security</td>
                                           <td>Supports authentication and authorization</td>
                                           <td>Enhanced security features, including support for SSL/TLS, JAAS</td>
                                           </tr>
                                           <tr>
                                           <td>Federation</td>
                                           <td>Requires custom configurations to set up federation</td>
                                           <td>Native support for geographically distributed clusters</td>
                                           </tr>
                                           <tbody>
                                           <table>


### Practical Applications

The choice between ActiveMQ and Artemis largely depends on the specific needs of your project and environment. For applications requiring traditional robust messaging with proven reliability, ActiveMQ remains very relevant. However, for newer applications demanding more flexibility, scalability, and modern features, Artemis might be the better option.

### Conclusion

Both Apache ActiveMQ and Artemis serve the fundamental purpose of efficient message brokering but cater to different scenarios. Understanding your project's requirements and future scalability needs can guide you in choosing the right tool for your architecture.
