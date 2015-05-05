# Statistics #

Statistics in the library exists at two points; per peer and per connection. The peer statistics contains statistics for all connections held by the peer and is available thru the property `NetPeer.Statistics`.
Per connection statistics is available thru the property `NetConnection.Statistics`.

By default statistics are only available in DEBUG builds. In `NetPeerStatistics.cs` and `NetConnectionStatistics.cs` there are defines at the top of the file you can uncomment to make statistics available in all builds.