---
name: wifi-manager
description: Manage Wi-Fi connections using wpa_supplicant or NetworkManager
---

# Wi-Fi Manager

Connect to and manage Wi-Fi networks on Linux-based Qualcomm boards using `nmcli` (NetworkManager) or `wpa_supplicant`.

## When to use this skill

Use this skill when you need to:
- Connect to a WPA2 or WPA3 protected network
- Scan for visible access points and check signal strength
- Configure a connection to persist across reboots
- Diagnose Wi-Fi link quality issues

## Usage

### Scan for nearby networks

```bash
nmcli dev wifi list
```

### Connect to a WPA2 network

```bash
nmcli dev wifi connect "MySSID" password "mypassword"
```

### Check the current connection status

```bash
nmcli con show --active
```

### Check signal strength (RSSI)

```bash
# Live monitoring every 2 seconds
watch -n 2 'iw dev wlan0 link'
```

### Make a connection start on boot

```bash
nmcli con modify "MySSID" connection.autoconnect yes
```

### Disconnect

```bash
nmcli dev disconnect wlan0
```

## wpa_supplicant alternative

For systems without NetworkManager, add your network to `/etc/wpa_supplicant/wpa_supplicant.conf`:

```
network={
    ssid="MySSID"
    psk="mypassword"
    key_mgmt=WPA-PSK
}
```

Then bring it up:

```bash
wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
dhclient wlan0
```

## Notes

- Replace `wlan0` with the actual interface name shown by `ip link`.
- For WPA3-SAE networks, use `key_mgmt=SAE` in the `wpa_supplicant` config.
