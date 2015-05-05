Three alterations are necessary to use Lidgren with the unity web player:

For Unity3D versions older than 3.0 comment this line:

`#define IS_STOPWATCH_AVAILABLE`

... in `NetTime.cs`.

For all versions comment this line:

`#define IS_MAC_AVAILABLE`

... in `NetPeer.Internal.cs` and...

`#define IS_FULL_NET_AVAILABLE`

... in `NetUtility.cs`