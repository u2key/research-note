# IEEE 802.3 Standards

| Standard                 | Max Speed | Cable Type              | Max Distance    |
| :--                      | :--       | :--                     | :--             |
| 802.3i  (10BASE-T)       | 10 Mbps   | Twisted Pairs (Cat.3+)  | Up to 100 m |
| 802.3u  (100BASE-TX)     | 100 Mbps  | Twisted Pairs (Cat.5)   | Up to 100 m |
| 802.3ab (1000BASE-T)     | 1 Gbps    | Twisted Pairs (Cat.5e+) | Up to 100 m |
| 802.3z  (1000BASE-SX/LX) | 1 Gbps    | Optical Fiber           | Up to 10 km |
| 802.3an (10GBASE-T)      | 10 Gbps   | Twisted Pairs (Cat.6a+) | Up to 100 m |
| 802.3ae (10GBASE-SR/LR)  | 10 Gbps   | Optical Fiber           | Over 10 km  |

## 1. About the Communication using Twisted-Pairs

### 1.1. About the Teisted-Pair cable

- The twisted-pair cables used to transmit data between devices.
- Differential signal is transferred using the twisted-pair cable.

### 1.2. About the Ethernet Cable

- The Ethernet cable consists of four-pair of twisted pair cable.
- There are two types.
  - Unshielded Twisted Pair Cable (UTP)
  - Shielded Twisted Pair Cable (STP)
- When four-pair cables are transmitting data, the delay may be different from one pair to another[1].
  - This is called propagation delay skew.

### 1.3. The Cause of the Propagation Delay Skew

- Construction of the Cable
  - In order to minimize crosstalk, all twisted-pair cables twisted rates are varied.
  - Industly standards require less than 50 ns for the delay skew.

### 1.4. The Cause of Errors to the communication

- Crosstalk often cause communication errors.

#### 1.4.1. Near-End Crosstalk (NEXT)

- A signal from the transmitting end (near end) of the same cable interferes with another pair.

#### 1.4.2. Power Sum Near-End Cross TAlk (PS-NEXT)

- Signals from multiple pairs at the near end of the same cable interfere with another pair.

#### 1.4.3. Far-End Cross Talk (FEXT)

- Signal from the near-end of the same cable interfare with another pair of far-end pair.
- Consideration must be given to full-duplex communication.

#### 1.4.4. Alien Crosstalk (ANEXT)

- Signal interference between nearby cables.

## References

[1] https://www.flukenetworks.com/blog/cabling-chronicles/101-series-getting-picture-delay-skew
