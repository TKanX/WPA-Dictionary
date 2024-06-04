# WPA-Dictionary

WPA Password Dictionary - **Used to crack Wifi passwords**.

Current size: **2GB**
Total number of passwords: **340 million**

### [Click here to download | 点我下载](https://github.com/TKanX/WPA-Dictionary/releases/download/Password-dictionary-ZIP-package/WPA-password-dictionary.zip)

# Get Wifi password through tools like Aircrack-ng

## Main idea

The user establishes a connection with the router through a password handshake packet containing hash encryption to confirm the identity. Then if you get the handshake packet, you can get the hash value of the password. Then by running a dictionary and comparing the hash value, you can get the plaintext of the password.

## Mac OS

### Install aircrack-ng

Install using macport

```shell
sudo port install aircrack-ng
```

### Packet capture - capture the handshake packet with password

MacBook comes with a wifi tool: Airport

1. First, disconnect from wifi

2. Check surrounding wifi

```shell
/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -s
```

3. Check the wireless network card device of this machine

```shell
ifconfig
```

PS: The default network card of MacOS is `en0`. If there is no external network card, use `en0`.

4. Capture packet

Airport can use the network card's listening mode to capture surrounding wireless network data packets. Among them, the most important data packet to us is: the packet containing the password - also called the handshake packet. When a new user or disconnected user automatically connects to wifi, a handshake packet will be sent. One attack method is reinjecting packet, which can force the wireless router to restart so that the handshake packet can be obtained when the user automatically connects. If there are many wireless router users, you can wait quietly.

```shell
sudo /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport en0 sniff 6
```

- `en0` is the wireless network card device
- `6` is the CHANNEL to crack the wifi.

5. Wait for the user to connect to wifi and get the handshake package.

Wait to obtain the handshake packet. The longer the packet capture time, the greater the chance of obtaining the handshake packet.

6. Stop capturing packets

```shell
^C
```

Captured packets are saved in `/tmp`

7. Crack wifi password

After obtaining the handshake package, we still need to crack the encrypted password.

A good password dictionary should include common weak passwords, mobile phone numbers, name and birthday combinations, passwords leaked by major websites, English words, etc. If it cannot be cracked using a dictionary, it means that the password is quite complex; brute force exhaustion is even more time-consuming and laborious. (On the importance of complex passwords).

PS: Various password dictionaries are available in the Github repository.

```shell
sudo aircrack-ng -w password.txt -b c8:3a:35:30:3e:c8 /tmp/
```

- `-w`：Specify dictionary file
- `－b`：Specify the wifi BSSID to be cracked

8. Crack results

`KET FOUND! [ ******* ]` means the password was successfully cracked. `[]` contains the cracked password.

Otherwise, change a password dictionary and try again.
