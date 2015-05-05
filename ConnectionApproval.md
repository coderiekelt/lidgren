# Connection approval #

Connection approval is a way for the server to accept or deny connections based on some data sent by the client.

The way this is done is that the client (or initiating peer) adds a `NetOutgoingMessage` to its Connect call, like this:

```
NetOutgoingMessage approval = myClient.CreateMessage();
approval.Write("secret");

myClient.Connect(host, 14242, approval);
```

This will send the string "secret" as approval data. On the server you must first enable connection approval:
```
myConfig.EnableMessageType(NetIncomingMessageType.ConnectionApproval);
```

... then, in your message loop, receive the appropriate message, read the data (and check other stuff, such as `SenderConnection.RemoveEndpoint` to block certain IPs.

```
switch (inc.MessageType)
{
	case NetIncomingMessageType.ConnectionApproval:
		string s = inc.ReadString();
		if (s == "secret")
			inc.SenderConnection.Approve();
		else
			inc.SenderConnection.Deny();
```