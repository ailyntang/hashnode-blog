## Networks and gateways 101

Gateway:
* Looks at requests coming through and decides if it will let it in or not
* Sometimes gateways will do load balancing. e.g. 5 hosts I could send this to. Load balancer and gateway are different concepts, but gateway can do both sometimes


Denial of service attacks gateways, and the end result is that clients will see 500s for "no access to application"

One method of getting around a DDOS is to switch gateways, so that the new gateway is not experiencing a DDOS.

Gateways are linked to W3 specifications, which are guidelines of what they expect
* e.g. One expectation is that the verbs, get, put, patch, etc. are all in uppercase
* Another expectation is that only a certain set of verbs are processed

So if the gateway receives a request which doesn't meet its expectations, it can deal with it in two ways
* Close the socket.
* Respond with a code, e.g. 503 bad gateway


Closing the socket: this means the gateway does not respond to the request. It closes the socket, which is like the connection / information request line.

Socket is like a port on the machine
* it's like a tunnel between two ends, which allows information to flow between the two
* Close the socket after the information is done
* If keeping an open connection the whole time, the radio can't turn off. Radios in mobiles are really expensive to keep on, so it's not good practice to keep a connection open.
* http requests have sockets and are pretty short lived
* sockets are lower layer than http
* technically can use a socket for multiple http connections, but depends on how things are implemented
* Each socket has a port, e.g. TCP port, which has a specific number attached to it
* One port can handle multiple sockets; every socket is linked to only one port
* Limitation is ~64,000 sockets per protocol
  * e.g. http is a protocol, which ensures that if any part is dropped, then there is an error, because a file needs all parts to work
  * e.g. udp is a protocol which is used for streaming video. Here, the protocol doesn't mind if parts are dropped, because video streaming can miss frames here and there without a big impact


Ports are linked to an IP address
* Typically a cable has an IP address
