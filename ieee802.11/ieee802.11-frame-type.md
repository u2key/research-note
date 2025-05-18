# IEEE 802.11 Frame Type 

## 1. **Management Frame** (00)

### 1.1. Announce AP beacons by using **Beacon Frame**

- In the 2.4 GHz band
  - Beacons are most commonly transmitted with DSSS modulation.

- In the 5 GHz band
  - Beacons are always transmitted with OFDM modulation.

- In a receiver circuit
  - DSSS modulation: 11 MHz sample rate
  - OFDM modulation: 20 MHz sample rate

### 1.2. Request a network information by using **Probe Request Frame**

### 1.3. Responce to Probe Request Frame by using **Probe Responce Frame**

### 1.4. Disconnect by using **Disassociation Frame**

### 1.5. Authenticate the communication target by using **Authentication Frame**

### 1.6. De-Authenticate the communication target by using **De-Authentication Frame**

### 1.7. Additional functions are realized by using **Action Frame**

---

## 2. **Control Frame** (01)

### 2.1. Confirm whether getiing ready for receiving or not by using **RTS Frame**

### 2.2. Announce transmittioning data by using **CTS Frame**

### 2.3. Announce correctly received data by using **Ack Frame**

### 2.4. Confirm some MAC frames by using **Block Ack Frame**

### 2.5. Request **Block Ack Frame** by using **Block Ack Request Frame**

---

## 3. **Data Frame** (10)

- Application specified data is stored on this frame.

