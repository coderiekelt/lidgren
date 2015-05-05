`WritePadBits` and `ReadPadBits` is an optimization option to reduce cpu usage for subsequent reads. Here is an example:

```
buffer.Write(true);
buffer.Write(false);
buffer.Write(true);
buffer.Write(largeByteArray); // a 128 byte large byte array
```

This code will write 3 bits (the 3 booleans) and then the large byte array. Unfortunately reading the large byte array will be much slower than necessary, since every byte in the array is displaced 3 or 5 bits. Putting in a `WritePadBits()` after the booleans will skip 5 bits of data before starting to write again; which will align the byte array and make reading it much faster.

```
buffer.Write(true);
buffer.Write(false);
buffer.Write(true);
buffer.WritePadBits();
buffer.Write(largeByteArray);
```

The only caveat is that you must remember to use `SkipPadBits()` when reading the data:

```
bool one = buffer.ReadBoolean();
bool one = buffer.ReadBoolean();
bool one = buffer.ReadBoolean();
buffer.SkipPadBits();
byte[] arr = buffer.ReadBytes(128);
```