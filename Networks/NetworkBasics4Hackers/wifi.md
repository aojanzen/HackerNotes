# WiFi Networks and Hacking #

(...)

## PKMID Attack
- New method to attack WPA2-PSK, discovered in August 2018 by the authors of `hashcat`
- Cracking of WPA2-PSK usually involves temporarily disconnecting a client from
  its host to capture the hash in the 4-way handshake upon reconnection.
- PMKID attack allows to get the password hash without the need to reconnect,
  saving significant time and effort.
- PMKID allows to retrieve the information for the brute-force password attack
  from only a single frame, the `RSN IE` frame.

PSK
  : Pre-shared Key

PMK
  : Pairwise Master Key, gemeinsamer Schlüssel bei 802.1x-Authentifizierung

EAPoL
  : Extensible Authentication Protocol over LAN,
  Netzwerk-Authentifizierungsprotokoll, das in IEEE 802.1x verwendet wird
    

### How It Works
Wireless adapter startup procedure:
1. Probe for known networks (SSIDs) to connect to
2. If in reach, AP will reply to probe with RSN (Robust Security network) response
3. Network adapter responds with Authentication Request (AR)
4. AR prompts AP to send its own authentication frames
5. WiFi adapter sends Association request with ESSID and RSN
6. AP responds with EAPoL frame that may contain the PMKID, which includes
  - PMK
  - PMK Name
  - AP's MAC address
  - Station's MAC address

All this information is hashed through the HMAC-SHA1-128 algorithm. The attack
is successful by grabbing the PMKID, stripping out all information but the
password hash, and running the hash through a hash cracker like `hashcat`.

The tools needed for the PMKID attack are
- `hcxdumptool`
- `hcxtools`

Both have to be installed installed in Kali with `sudo apt install ...`.

Probe available APs for the PMKID with `sudo hcxdumptool -i wlan0 -w
dumpfile.pcapng`.

`hcxdumptool`is capable of pulling the PMKID from many WiFi APs. It may not be
able to pull all of them, but it can usually pull 80-90%.

