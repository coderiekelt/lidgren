Using UPnP you can easily forward ports on your router to allow incoming connections thru NAT. Just enable UPnP in the `NetPeerConfiguration` and call `NetPeer.UPnP.ForwardPort()`.

```
myPeerConfig.EnableUPnP = true;

// attempt to forward port 14242
myPeer.UPnP.ForwardPort(14242, "Text detail here");
```