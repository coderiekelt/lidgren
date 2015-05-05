# Peer/server discovery #

Peer discovery is the process of clients detecting what servers are available. Discovery requests can be made in two ways; locally as a broadcast, which will send a signal to all peers on your subnet. Secondly you can contact an ip address directly and query it if a server is running.

Responding to discovery requests is done in the same way regardless of how the request is made.

Here's how to do on the client side; ie. the side which makes a request:

```
// Enable DiscoveryResponse messages
config.EnableMessageType(NetIncomingMessageType.DiscoveryResponse);
 
// Emit a discovery signal
Client.DiscoverLocalPeers(14242);
```

This will send a discovery signal to your subnet; Here's how to receive the signal on the server side, and send a response back to the client:

```
// Enable DiscoveryRequest messages
config.EnableMessageType(NetIncomingMessageType.DiscoveryRequest);
 
// Standard message reading loop
NetIncomingMessage inc;
while ((inc = Server.ReadMessage()) != null)
{
    switch (inc.MessageType)
    {
        case NetIncomingMessageType.DiscoveryRequest:
 
            // Create a response and write some example data to it
            NetOutgoingMessage response = Server.CreateMessage();
            response.Write("My server name");
 
            // Send the response to the sender of the request
            Server.SendDiscoveryResponse(response, inc.SenderEndpoint);
            break;
```

When the response then reaches the client, you can read the data you wrote on the server:

```
// Standard message reading loop
NetIncomingMessage inc;
while ((inc = Client.ReadMessage()) != null)
{
    switch (inc.MessageType)
    {
        case NetIncomingMessageType.DiscoveryResponse:
 
            Console.WriteLine("Found server at " + inc.SenderEndpoint + " name: " + inc.ReadString());
            break;
```