## RX pattern

PRomises are good but they aren't able to be cancelled and you may run into uenxpected orders from async nature of the calls

RX is there to try and fix those issues. RX in the terms of the Observable pattern (or in Swift, called publisher / subscriber)
four events:
* Subscribed - always has this
* next - zero or more
* completed - this may never happen. e.g. an obvserable with a web socket that will never complete, because it sends data over time forever
* error - this may never happen. e.g. an obvserable with a web socket that will never complete, because it sends data over time forever


Convention for an observable is to add a $ as the suffix. e.g. `timer$`


Can use the unsubscribe function to do some clean up. E.g. stop looking at web sockets
* It's up to the subscriber to call unsubscribe. In some instances, unsubscribe happens automaticcaly with garbage collection and the deinit.
* Clean up memory
* Stop behaviours that you no longer need


observer = subscriber

observer.next: let the observer know something has happened. e.g. recieved a new chat in a message app
Then in the const subscription, the `value` responds to receivin gthe next event. So you might want to present the chat.

You can add more pipes to the original observable

Most interesting part is that you can return an observable as part of the 'next' call.
THIS is the part that helps with unexpected async stuff.
This does my head in a bit. Have to figure it out - look at Rx Swift documentation.


Analogies: turn on the water in the pipe. Observables are pipes. Can add more pipes on top of each other.

every chained method is a new pipe that you are adding on. So you can have lots of complex pipes.

`debounce(200)` waits 2ms, then takes the most recent action and does something
`throttle(200)` takes the very first call, then waits 2ms before executing. So if other more relevant calls happen in that time, they are ignored. 
Use throttle for something like same API call is made eachtime.
Use debounce for seomthing like search API call, where you are entering key strokes and registering eveyr new key stroke in one observable. YOu want to wait until there is a 2ms pause in key strokes, then take ALL the key strokes (whereas throttle would take only the first key stroke so you would enver get to search the whole word, only the frist letter)

DispatchQueue.gloabal() is background thread
DispatchQueue.main is foreground thread

