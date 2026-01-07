# WirelessConnect: Secure Long-Range Communication (up to 5 km)

Decentralized, encrypted messaging over LoRa radio technology - communicate securely without internet, cellular networks, or third-party infrastructure.

## Features

- **Decentralized**: No internet or cellular towers required
- **Secure**: Hardware-level AES-128 encryption for all messages
- **Reliable**: Built-in acknowledgment (ACK) protocol for message confirmation
- **Long Range**: Up to 5 km communication distance
- **Cross-Platform**: Runs on Raspberry Pi 5 (optional, for display) and Windows/Linux (sender/receiver)

## Hardware Requirements

### Receiver (Optional - Display Only)
- Raspberry Pi 5 (optional for displaying received messages on a monitor)
- Display monitor
- SX1262 LoRa module

### Sender/Receiver (Required)
- Desktop or laptop (Windows/Linux)
- SX1262 USB-to-LoRa module

**Note**: The sender application functions as both a transmitter and receiver. You can send and receive messages directly from your laptop/desktop. A Raspberry Pi 5 is only needed if you want messages displayed on a separate monitor. For point-to-point communication, two laptops with LoRa devices are sufficient—one laptop connected to the LoRa device can use the sender application to communicate with another.

**Hardware Reference**: [USB to LoRa Data Transfer Module (SX1262)](https://www.waveshare.com/usb-to-lora.htm)

## Installation Guide

### Receiver Setup (Optional - Raspberry Pi 5 Display)

1. Insert the SX1262 LoRa module into the Raspberry Pi 5
2. Connect to the device via SSH and run the following command to download and install:

```bash
wget -4 https://raw.githubusercontent.com/ghostidentity/wirelessconnect/main/receiver/installer.deb -O /tmp/installer.deb && sudo dpkg -i /tmp/installer.deb
```

3. **Security Configuration**: The encryption key defaults to `1234`. After installation, update it at `/opt/sysrefiners/config` for production use

4. **Sending Messages from Receiver**: The receiver includes a sender executable at `/opt/sysrefiners/` that allows you to send messages directly. Use the following syntax:

```bash
sender "your message here"
```

This is useful for testing or sending messages directly from the Raspberry Pi without needing a separate sender device.

### Sender/Receiver Setup (Windows/Linux - Main Communication Device)

1. **Connect the LoRa Device**: Plug the SX1262 USB-to-LoRa module into your computer
2. Locate the appropriate executable in the `sender/` folder for your OS
3. Copy the `config.xml` file to the same directory as the executable
4. Run the executable and look for the connection status button (initially shows "DISCONNECTED")
5. Click the status button to connect to the LoRa device. Once connected, it will turn green showing "CONNECTED"
6. To disconnect, click the green status button again

**Understanding Message Acknowledgement:**
- When you send a message, the status will show "sent - waiting for acknowledgement"
- If you receive an acknowledgement (shown as "message acknowledged"), this confirms that the receiver successfully decrypted and received your message
- No acknowledgement means the message was sent but the receiver either didn't receive it or couldn't decrypt it

**Multi-Laptop Setup**: Two laptops can communicate directly with each other. Simply connect a LoRa device to each laptop and run the sender application on both. Both laptops will send and receive messages simultaneously using the same encryption key.

## Configuration

Update the `config.xml` file with your preferred settings. The file contains the following structure:

```xml
<Config>
    <HistoryPath>history.txt</HistoryPath>
    <EncryptionKey>1234</EncryptionKey>
    <Source>device</Source>
    <AcceptFrom>desktop</AcceptFrom>
</Config>
```

**Configuration Parameters:**
- **HistoryPath**: Path to the file where message history is stored
- **EncryptionKey**: The key used to encrypt/decrypt messages. Must be identical on sender and receiver. Change from default `1234` to a secure value, but ensure its less than 65000
- **Source**: The identifier for this device when sending messages
- **AcceptFrom**: Specifies which senders this device will accept messages from. Use `ANYONE` to accept from any sender with matching encryption key

```
wirelessconnect/
├── README.md
├── receiver/          # Receiver application for Raspberry Pi
├── sender/            # Sender applications
│   ├── windows/       # Windows executable and config
│   └── linux/         # Linux executable and config
└── screenshots/       # UI reference images
```

## Important Notes

- **Frequency**: The LoRa device has been tested to operate on the 433 MHz frequency
- **Line of Sight**: For optimal range and reliability, position the device with clear line of sight to the receiver
- **Encryption Key Synchronization**: The encryption key must be identical on both sender and receiver to decrypt messages successfully
- **Message Filtering**: Use the `<AcceptFrom>` option in `config.xml` to filter incoming messages:
  - Set to `<AcceptFrom>ANYONE</AcceptFrom>` to accept messages from any sender (provided encryption keys match)
  - Set to a specific name (e.g., `<AcceptFrom>Mark</AcceptFrom>`) to only accept messages from that sender. The sender's "source" value must match this setting
- **Screenshots**: Check the `screenshots/` folder for UI reference images

## Troubleshooting

- **Device not detected**: Ensure the LoRa module is properly plugged in and recognized by your OS
- **Messages not received**: Verify both devices have identical encryption keys
- **Connection fails**: Check that the sender and receiver `source`/`AcceptFrom` settings match
- **Poor range**: Reposition devices for line-of-sight communication and reduce obstacles


## Known Issue
- The sender app automatically selects the COMPORT associated to lora device connected to windows, unfortunately if the bluetoth is enabled, it fails to select the right one. Thus, you need to disable the bluetooth before you connect.

## About

Developed by **Mark Tagab**

*Empowering secure communication through decentralized, open technology.*
