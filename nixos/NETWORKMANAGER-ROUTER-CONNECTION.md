# NixOS: connect to a router over Ethernet without losing Wi-Fi

## Goal and assumptions

Configure an Ethernet interface for router administration while retaining Wi-Fi as the internet connection. Commands assume NetworkManager manages the interfaces and use `enp2s0` as an **example** Ethernet device. Confirm your actual device name first.

> These are proposed procedures; verify on the target laptop and router before marking them tested. Avoid changing Wi-Fi profiles, global routes, or firewall rules.

## 1. Inspect interfaces and routing

```bash
nmcli device status
nmcli connection show
ip -4 address
ip -4 route
ip -4 route get 1.1.1.1
```

Identify the Ethernet interface (example: `enp2s0`) and Wi-Fi interface. The `default via ... dev ...` route should point to Wi-Fi. Substitute the actual Ethernet interface below.

## 2. DHCP profile: ethernet-auto

For routers with DHCP enabled:

```bash
ETH=enp2s0
nmcli connection show ethernet-auto >/dev/null 2>&1 || \
  sudo nmcli connection add type ethernet ifname "$ETH" con-name ethernet-auto
sudo nmcli connection modify ethernet-auto \
  connection.interface-name "$ETH" \
  ipv4.method auto \
  ipv4.never-default yes \
  ipv4.ignore-auto-dns yes \
  ipv6.method disabled \
  connection.autoconnect yes
sudo nmcli connection up ethernet-auto ifname "$ETH"
```

`ipv4.never-default yes` prevents this profile from supplying an IPv4 default route; `ipv4.ignore-auto-dns yes` avoids using the router's DHCP DNS servers. Disabling IPv6 on this Ethernet profile prevents an unintended IPv6 default route. Wi-Fi remains separately managed.

Inspect the assigned address and routes:

```bash
ip -4 addr show dev "$ETH"
ip -4 route
ip -4 route get 1.1.1.1
nmcli -f GENERAL,IP4 device show "$ETH"
```

The Ethernet link should have a local subnet route; the internet default route should still use Wi-Fi.

## 3. Static profile: router-config

Use this if DHCP is unavailable. **First identify the router's management subnet** (from its label, manual, or prior configuration). The following example assumes router IP `192.168.1.1` and laptop IP `192.168.1.10/24`. Do not blindly use these addresses on an unknown network.

```bash
ETH=enp2s0
nmcli connection show router-config >/dev/null 2>&1 || \
  sudo nmcli connection add type ethernet ifname "$ETH" con-name router-config
sudo nmcli connection modify router-config \
  connection.interface-name "$ETH" \
  ipv4.method manual \
  ipv4.addresses 192.168.1.10/24 \
  ipv4.gateway "" \
  ipv4.dns "" \
  ipv4.never-default yes \
  ipv6.method disabled \
  connection.autoconnect no
sudo nmcli connection up router-config ifname "$ETH"
```

To use a different router subnet, change `ipv4.addresses` accordingly. The example assumes no address conflict with Wi-Fi or other interfaces; if the Wi-Fi and router LAN use the **same subnet**, overlapping routes may cause ambiguity. Prefer a temporary isolated Wi-Fi connection, a different router LAN subnet, or a more advanced policy-routing plan rather than assuming this recipe will work.

## 4. Test router reachability

```bash
ping -c 3 192.168.1.1
ip -4 route get 192.168.1.1
ip -4 route get 1.1.1.1
```

Replace `192.168.1.1` with the actual router IP. A failed ping does not always imply the web interface is unavailable (ICMP may be blocked). Open `http://192.168.1.1` or `https://192.168.1.1` as appropriate. Never assume a particular firmware's default IP.

## 5. Switch profiles / recover

```bash
# Switch back to DHCP
sudo nmcli connection up ethernet-auto ifname enp2s0

# Switch to static addressing
sudo nmcli connection up router-config ifname enp2s0

# Disconnect Ethernet only, leaving Wi-Fi alone
sudo nmcli device disconnect enp2s0

# Restore DHCP Ethernet profile after a manual session
sudo nmcli connection up ethernet-auto ifname enp2s0
```

NetworkManager generally activates one profile at a time on an interface. To inspect the active profile:

```bash
nmcli -f GENERAL.CONNECTION,GENERAL.DEVICE device show enp2s0
nmcli connection show --active
```

If the internet route unexpectedly changes, inspect `ip -4 route` and `ip -6 route` before editing anything. Do not delete or reset the Wi-Fi connection to fix Ethernet issues.

## 6. Checklist

- [ ] Actual Ethernet interface name verified
- [ ] Wi-Fi online before connecting router
- [ ] Router subnet confirmed; no overlap with Wi-Fi subnet
- [ ] Ethernet profile has `ipv4.never-default yes`
- [ ] `ip -4 route get 1.1.1.1` still selects Wi-Fi
- [ ] Router management IP reachable
- [ ] Router configuration backed up before changes

## Notes

Commands use standard `nmcli` and `ip` utilities available in a typical NixOS NetworkManager setup. If `nmcli` is unavailable, confirm that NetworkManager is enabled in NixOS configuration rather than making ad hoc system-wide networking changes. The profile names are conventions for this project.
