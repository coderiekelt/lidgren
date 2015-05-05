# Unconnected messages #

Normally when using the library you will create a connection and then start sending messages, but you don't have to. Lidgren can also send unconnected messages. This is how:

```
NetOutgoingMessage msg = client.CreateMessage();
msg.Write("Hello stranger!");

IPEndPoint receiver = new IPEndPoint(NetUtility.Resolve("host.somewhere.com"), 1234);

client.SendUnconnectedMessage(msg, receiver);
```

This will send an unconnected message to host.somewhere.com port 1234. The receiver must be started and have the unconnected data message type enabled, like this:

```
config.SetMessageTypeEnabled(NetIncomingMessageType.UnconnectedData, true);
```

When a message arrives; the sender can be gotten from `msg.SenderEndpoint`.