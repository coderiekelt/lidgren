# Message encryption #

Messages can be encrypted using the Xtea, or other algorithms. This is done like this:

```
INetEncryption algo = new NetXtea("TopSecret");

NetOutgoingMessage msg = client.CreateMessage();
msg.Write("Some content");
msg.Write(42);
msg.Encrypt(algo);
```

Notice that encryption must be done _last_ and that after encryption no more data should be written to the message.

Decrypting the message on arrival is done like this:

```
INetEncryption algo = new NetXtea("TopSecret");

incomingMessage.Decrypt(algo);
```

The algorithm instance can be reused over the application lifetime, but it is not thread safe; so you'll have to create an instance per thread if you're encrypting/decrypting on more than one thread.

Note that this feature does not concern itself with synchronizing the password between hosts and it only encrypts the content of the message, not message headers.

Currently Xtea, AES, DES, Triple DES, RC2 and a simple XOR algorithm are implemented; it's easy to extend; any class implementing the `INetEncryption` can be used.
For block level algorithms there is a base class, `NetBlockEncryption`, that can be used to simplify such ciphers - you will only need to provide implementations for `EncryptBlock`, `DecryptBlock` and `BlockSize`.