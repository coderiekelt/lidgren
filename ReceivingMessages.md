There are three different ways of consuming messages received by the library.

1. **`Polling`**

If you already have an application loop for your game this would be the most suitable to use; just call `ReadMessage()` until it returns null.

```
NetIncomingMessage msg;
while ((msg = peer.ReadMessage()) != null)
{
   // process message here
}
```


2. **`Callback`**

If your application is completely event/UI-driven and does not have an application loop per se, you can register a callback to fire when a message arrives. The registration must happen from the correct SynchronizationContext (ie. call RegisterReceivedCallback from the same thread you want to receive the message notification on)

```
peer.RegisterReceivedCallback(new SendOrPostCallback(GotMessage)); 

public static void GotMessage(object peer)
{
	var msg = peer.ReadMessage();
	// process message here
}	
```

3. **`EventWaitHandle`**

If your incoming message processing is run on a separate thread you can use the `MessageReceivedEvent` property to have your thread block until an incoming message arrives.

```
// in your separate thread
peer.MessageReceivedEvent.WaitOne(); // this will block until a message arrives

var msg = peer.ReadMessage();
// process message here
```