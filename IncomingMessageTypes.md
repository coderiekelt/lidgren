# Incoming message types #

  * **`Error`** - This should never appear, it indicates a corrupt message.

  * **`StatusChanged`** - Contains a single byte which can be cast to a `NetConnectionStatus` enum, then a human readable string explaining the reason for the change in status. These messages will only be received for your own connection(s); not other connections to the same server (if client).

  * **`Data`** - Contains the data written by a connected sender, available in the `SenderConnection` property of the message.

  * **`UnconnectedData`** - Contains data written by a host that is not connected, this message type is disabled by default. Sender ip endpoint is available in the `SenderEndpoint` property of the message.

  * **`ConnectionApproval`** - Contains the data passed to `Connect()` on the remote side. The application should call `Approve()` or `Deny()` on the connection object in `SenderConnection` of the message. This message type is disabled by default.

  * **`Receipt`** - Not yet implemented.

  * **`DiscoveryRequest`** - See [Discovery](http://code.google.com/p/lidgren-network-gen3/wiki/Discovery).

  * **`DiscoveryResponse`** - See [Discovery](http://code.google.com/p/lidgren-network-gen3/wiki/Discovery).

  * **`VerboseDebugMessage`** - Contains a single string of diagnostics. Only available in DEBUG builds and disabled by default.

  * **`DebugMessage`** - Contains a single string of diagnostics. Only available in DEBUG builds.

  * **`WarningMessage`** - Contains a single string of diagnostics.

  * **`ErrorMessage`** - Contains a single string of diagnostics.

  * **`NatIntroductionSuccess`** - NAT traversal succeeded.

  * **`ConnectionLatencyUpdated`** - Means the average latency for the connection has been updated

Message types can be enabled or disabled in the `NetPeerConfiguration` using `EnableMessageType()` or `DisableMessageType()` methods.