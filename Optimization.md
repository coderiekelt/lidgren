# How to optimize Lidgren #

  * Recycle incoming messages. This will put less strain on the garbage collector and thus faster GC sweeps.

  * Preallocate size in `NetOutgoingMessage` by using the `CreateMessage(int)` method. This will remove the need for the library to resize the buffer as you add data to it - relieving GC pressure.

  * Disable unused message types. This will reduce the amount of unused messages created which reduces both GC pressure and cpu time to create them.

  * Tweak the `NetPeerConfiguration` values. They can help with latency, bandwidth usage and memory usage.

  * To save on bandwidth; use the smallest data type necessary and consider using `Write(value, numbits)`, `WriteVariableInt32()`, `WriteRangedInteger()`, `WriteUnitSingle()` etc to encode your data.

  * Use unreliable channels for data that does not absolutely need to be reliable. This will reduce the amount of memory used to store not-yet-acknowledged messages. It will also reduce the amount of cpu used to track reliable messages.

  * Use `WritePadBits` to speed up reading; see WritePadBits