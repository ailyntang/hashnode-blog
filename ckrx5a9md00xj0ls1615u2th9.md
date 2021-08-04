## API calls: data vs params

The norm is that with `GET` calls, clients should send the server parameters as part of the route. 
For example, to send the server the parameter `"flavour": String`, it would be  `GET api/cake?flavour=chocolate`

However for a `POST`, clients should send the server the parameters as part of the http body, as a JSON