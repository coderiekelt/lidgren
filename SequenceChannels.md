There is an override for `SendMessage()` which takes an integer called 'sequenceChannel' - this can be used for certain delivery methods, namely `UnreliableSequenced`, `ReliableSequenced` and `ReliableOrdered`.

The sequence channel is a number between 0 and (`NetConstants.NetChannelsPerDeliveryMethod - 1`) - currently 31. The reason for this limitation is to reduce the amount of overhead per message. Note that there are this amount of channels **per delivery method**, not in total.

Messages sent in a certain sequence channel will be dropped/withheld independently of messages sent in a different sequence channel.

An example:

```
Lets say 'Health' are sent using the delivery method ReliableSequenced.
This means that if a health message is delayed so that message #7 arrives
AFTER message #8 has already arrived... #7 will be dropped (since #8 has
already arrived with fresher information).

This works well, but things is complicated if we also send 'Ammo' as
ReliableSequenced. Lets say Ammo is sent in message #7 but message #8,
containing 'Health' arrives before it - then #7 will be dropped for the
same reasons as above.

This is clearly not what we want; #7 still contains the freshest info we
have on ammo.

We solve this by using different sequence channels for health and ammo
messages. Message numbers are individual per sequence channel, so health
and ammo message cannot clash.
```

Code:

```
// create a message
NetOutgoingMessage om = peer.CreateMessage();
om.Write("Hello internet");

// send it as Reliable Sequenced channel number 8
peer.SendMessage(om, connection, NetDeliveryMethod.ReliableSequenced, 8);
```