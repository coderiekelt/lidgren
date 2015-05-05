## General features ##
  * Very easy to use
  * Low CPU overhead
  * Supports any number of connections

## Reliability ##
  * Duplicate packets detected and dropped for all delivery methods (even unreliable)
  * Sequenced messages, late packets are dropped
  * Reliable messages, lost packets are resent
  * Ordered reliable messages, lost packets are resent and early packets are withheld
  * Multiple channels for both sequenced and ordered messages
  * Message fragmentation; large messages sent using many packets transparent to the application

## Low bandwidth ##
  * Very compact packet layout
  * Message coalescing; multiple messages automatically sent in as few packets as possible
  * Bitstream write/read
  * Compression methods for ranged integers and floats
  * Compact message storage; reliable messages will be stored just once for broadcasts

## Utility features ##
  * Clock synchronization; reliable packet timestamps regardless of packet roundtrip time variations
  * Optional message encryption
  * Local network server discovery
  * Peer and connection statistics
  * Simple port forwarding using UPnP
  * NAT traversal support
  * Ability to simulate lag, loss and packet duplication for testing
