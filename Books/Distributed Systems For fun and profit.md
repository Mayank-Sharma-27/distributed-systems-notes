
The Writings are from here : https://book.mixu.net/distsys/single-page.html

Can glance over it not deep

Distributed programming is the art of solving the same problem that you can solve on a single computer using multiple computers - usually, because the problem no longer fits on a single computer.

### **[Scalability](https://en.wikipedia.org/wiki/Scalability)**


is the ability of a system, network, or process, to handle a growing amount of work in a capable manner or its ability to be enlarged to accommodate that growth.

A scalable system is one that continues to meet the needs of its users as scale increases. There are two particularly relevant aspects - performance and availability - which can be measured in various ways.

### Performance (and latency)

is characterized by the amount of useful work accomplished by a computer system compared to the time and resources used.

Depending on the context, this may involve achieving one or more of the following:

- Short response time/low latency for a given piece of work
- High throughput (rate of processing work)
- Low utilization of computing resource(s)

Latency

The state of being latent; delay, a period between the initiation of something and the occurrence

And what does it mean to be "latent"?

Latent

From Latin latens, latentis, present participle of lateo ("lie hidden"). Existing or present but concealed or inactive.

### Availability (and fault tolerance)

[Availability](https://en.wikipedia.org/wiki/High_availability)

the proportion of time a system is in a functioning condition. If a user cannot access the system, it is said to be unavailable.

Formulaically, availability is: `Availability = uptime / (uptime + downtime)`.

Fault tolerance

ability of a system to behave in a well-defined manner once faults occur

**Distributed systems are constrained by two physical factors:**

- the number of nodes (which increases with the required storage and computation capacity)
- the distance between nodes (information travels, at best, at the speed of light)

## Design techniques: partition and replicate

The manner in which a data set is distributed between multiple nodes is very important. In order for any computation to happen, we need to locate the data and then act on it.