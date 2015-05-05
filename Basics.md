Lidgren network library is all about messages. There are two types of messages:

**Library messages** telling you things like a peer has connected or diagnostics messages (warnings, errors) when unexpected things happen.

**Data messages** which is data sent from a remote (connected or unconnected) peer.

The base class for establishing connections, receiving and sending message is the `NetPeer` class. You can use it to make a peer-to-peer network, but if you are creating a server/client topology there are special classes called `NetServer` and `NetClient`. They inherit `NetPeer` but set some defaults and include some helper methods/properties.

Here's how to set up a `NetServer`:

```
NetPeerConfiguration config = new NetPeerConfiguration("MyExampleName");
config.Port = 14242;
 
NetServer server = new NetServer(config);
server.Start();
```

The code above first creates a configuration. It has lots of properties you can change, but the default values should be pretty good for most applications. The string you provide in the constructor (`MyExampleName`) is an identifier to distinquish it from other applications using the lidgren library. Just make sure you use the same string in both server and client - or you will be unable to communicate between them.

Secondly we've set the local port the server should listen to. This is the port number we tell the client(s) to connect to. The local port can be set for a client too, but it's not needed and not recommended.

Thirdly we create our server object and fourth we `Start()` it. Starting the server will create a new network thread and bind to a socket and start listening for connections.

Early on we spoke about messages; now is the time to start receiving and sending some. Here's a code snippet for receiving messages:

```
NetIncomingMessage msg;
while ((msg = server.ReadMessage()) != null)
{
    switch (msg.MessageType)
    {
        case NetIncomingMessageType.VerboseDebugMessage:
        case NetIncomingMessageType.DebugMessage:
        case NetIncomingMessageType.WarningMessage:
        case NetIncomingMessageType.ErrorMessage:
            Console.WriteLine(msg.ReadString());
            break;
        default:
            Console.WriteLine("Unhandled type: " + msg.MessageType);
            break;
    }
    server.Recycle(msg);
}
```

So, lets dissect the above code. First we declare a `NetIncomingMessage`, which is the type of incoming message. Then we read a message and handle it, looping back as long as there are messages to fetch. For each message we find, we switch on something called `MessageType` - it's a description what the message contains. In this code example we only catch messages of type `VerboseDebugMessage`, `DebugMessage`, `WarningMessage` and `ErrorMessage`. All those four types are emitted by the library to inform about various events. They all contains a single string, so we use the method `ReadString()` to extract a copy of that string and print it in the console.

Reading data will increment the internal message pointer so you can read subsequent data using the `Read*()` methods.

For all other message types we just print that they're currently unhandled.

Finally, we recycle the message after we're done with it - this will enable the library to reuse the object and create less garbage.

Sending messages are even easier:

```
NetOutgoingMessage sendMsg = server.CreateMessage();
sendMsg.Write("Hello");
sendMsg.Write(42);
 
server.SendMessage(sendMsg, recipient, NetDeliveryMethod.ReliableOrdered);
```

The above code first creates a new message, or uses a recycled message, which is why it's not possible to just create a message using new(). It then writes a string ("Hello") and an integer (System.Int32, 4 bytes in size) to the message.

Then the message is sent using the `SendMessage()` method. The first argument is the message to send, the second argument is the recipient connection - which we won't go into detail just yet - and the third argument are HOW to deliver the message, or rather how to behave if network conditions are bad and a packet gets lost, duplicated or reordered.

There are five delivery methods available:

| `Unreliable` | This is just UDP. Messages can be lost or received more than once. Messages may not be received in the same order as they were sent. |
|:-------------|:-------------------------------------------------------------------------------------------------------------------------------------|
| `UnreliableSequenced` | Using this delivery method messages can still be lost; but you're protected against duplicated messages. If a message arrives late; that is, if a message sent after this one has already been received - it will be dropped. This means you will never receive "older" data than what you already have received. |
| `ReliableUnordered` | This delivery method ensures that every message sent will be eventually received. It does not however guarantee what order they will be received in; late messages may be delivered before newer ones. |
| `ReliableSequenced` | This delivery method is similar to `UnreliableSequenced`; except that it guarantees that SOME messages will be received - if you only send one message - it will be received. If you sent two messages quickly, and they get reordered in transit, only the newest message will be received - but at least ONE of them will be received. |
| `ReliableOrdered` | This delivery method guarantees that messages will always be received in the exact order they were sent. |

Here's how to read and decode the message above:

```
NetIncomingMessage incMsg = server.ReadMessage();
string str = incMsg.ReadString();
int a = incMsg.ReadInt32();
```