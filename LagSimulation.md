# Simulating bad network conditions #

On the internet, your packets are likely to run into all kinds of trouble. They can be delayed and lost and they might even arrive multiple times at the destination. Lidgren has a few options to simulate how your application or game will react when this happens.
They are all configured using the `NetPeerConfiguration` class - these properties exists:

| `SimulatedLoss` | This is a float which simulates lost packets. A value of 0 will disable this feature, a value of 0.5f will make half of your sent packets disappear, chosen randomly. Note that packets may contain several messages - this is the amount of packets lost. |
|:----------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `SimulatedDuplicatesChance` | This is a float which determines the chance that a packet will be duplicated at the destination. 0 means no packets will be duplicated, 0.5f means that on average, every other packet will be duplicated. |
| `SimulatedMinimumLatency, SimulatedRandomLatency` | These two properties control simulating delay of packets in seconds (not milliseconds, use 0.05 for 50 ms of lag). They work on top of the actual network delay and the total delay will be: Actual one way latency + `SimulatedMinimumLatency` + (randomly per packet 0 to `SimulatedRandomLatency` seconds) |

It's recommended to assume symmetric conditions and configure server and client with the same simulation settings.

Simulating bad network conditions only works in DEBUG builds.