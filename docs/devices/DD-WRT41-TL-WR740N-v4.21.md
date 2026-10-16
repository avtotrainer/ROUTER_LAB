# DD-WRT41 — TP-Link TL-WR740N v4.21 configuration handout

**Record date:** 2026-10-16  
**Device label:** DD-WRT41 (user designation)  
**Model / hardware revision:** TP-Link TL-WR740N / Ver: 4.21 (reported from label)  
**Target role:** School wireless access point, analogous to DD-WRT51  
**Status:** User reports Wi-Fi settings changed; final applied/runtime/site tests pending

## 1. Work completed and evidence

1. The router was factory-reset before this session (user report).
2. A DD-WRT web interface was identified at `192.168.1.1`. The NixOS laptop obtained `192.168.1.196/24` over Ethernet from DHCP server `192.168.1.1`; ping to `192.168.1.1` succeeded (3/3).
3. User reported upgrading the installed DD-WRT firmware and restoring a configuration backup. **Firmware build, uploaded image filename, backup origin, compatibility and backup storage location were not recorded.** Restoring a backup from another device/build may be unsafe; do not repeat or rely on it without verification.
4. User identified the router as `10.107.143.41` and reported working Web UI access. This is the intended management IP, not an independently verified runtime CLI result.
5. On NixOS, `router-config` was activated and then modified to `ipv4.addresses 10.107.143.101/24`. Because modification followed activation, the new address's activation was not independently confirmed; Web UI access was reported working.
6. Screenshots of Wireless Basic Settings after restore showed: `wlan0` AP, SSID `TetriLab`, regulatory domain `GEORGIA`, N/G Mixed, 20 MHz, Auto channel, Bridged, WMM enabled, AP Isolation disabled, Antenna Gain 0 dBi, and TX Power field 20 dBm with UI note Max 18.
7. User stated that wireless settings were changed afterward. No final screenshot, `Save`/`Apply Settings` confirmation, or runtime wireless output was supplied.

## 2. Planned wireless settings

| Setting | Target / note |
|---|---|
| SSID | `TetriLab` (replaces an existing AP of this name at deployment site) |
| Radio Mode | AP |
| Network Mode | Mixed |
| Channel Width | 20 MHz |
| Channel | 6, provisional based on one local scan |
| Country / Domain | GEORGIA |
| Network Configuration | Bridged |
| WMM | Enabled |
| AP Isolation | Disabled |
| Security | WPA2-PSK / AES, **not yet verified** |
| Antenna Gain | 5 dBi **only if antenna specification is confirmed** |
| TX Power | 13 dBm suggested starting point; applied value unverified |

The initial scan from the NixOS laptop reported:
- `MES` — BSSID `F8:D1:11:9E:40:8A`, channel 1, signal 100.
- Existing `TetriLab` — BSSID `14:EB:B6:BD:47:D8`, channel 2, signal 93; intended to be replaced.
- `TETRI WYLEBI` — channel 11, signal 42.
- `FreeNet - tetrcylebi 1` — channel 7, signal 24.

Channel 6 is a starting choice, not a measured interference or throughput optimum. Rescan after removing the old TetriLab AP and at the actual deployment location.

## 3. NixOS management workstation

Observed: Ethernet `enp2s0`; Wi-Fi `wlp0s20f3` connected to `TetriLab` during initial diagnostics. The initial route to `1.1.1.1` used Wi-Fi. The automatic Ethernet profile `ethernet-auto` had `ipv4.never-default yes`.

Commands already reported:
```bash
sudo nmcli connection up router-config
sudo nmcli connection modify router-config ipv4.addresses 10.107.143.101/24
```

To verify current state without altering settings:
```bash
nmcli device status
ip -4 addr show enp2s0
ip route get 10.107.143.41
ip route get 1.1.1.1
```

## 4. Deployment topology and outstanding checks

Target topology follows the DD-WRT51 AP/bridge design: unique static management IP; upstream DHCP (not local router DHCP); no WAN NAT/routing; LAN/Wi-Fi bridge; physical WAN uplink bridged **only after confirming actual port mapping**. Do not assume the previous device's bridge members or site gateway apply here.

**Pending:**
1. Record firmware version/build, physical MACs, radio BSSID, and actual backup filename/location.
2. Confirm `10.107.143.41/24` is configured and unique; check management access after reboot.
3. Verify WPA2-PSK/AES without documenting passwords.
4. Check DHCP server and DNSMasq disabled, WAN/NAT disabled, and actual bridge membership/physical WAN port mapping.
5. Verify radio runtime with `iw dev wlan0 info` and `ps | grep '[h]ostapd'`; a saved NVRAM setting does not prove AP mode.
6. At the deployment site, confirm clients associate to the correct BSSID and receive upstream DHCP, gateway, DNS, and internet connectivity.
7. Perform reboot and client acceptance tests; record results and rollback plan.

## 5. Risks and rollback

The user restored a backup after a firmware upgrade; the source and compatibility of that backup are unknown. Do not blindly restore it again or flash another image. Before further major changes, capture the current working configuration, identify the firmware/build, verify device-specific recovery options, and preserve Ethernet Web UI access. No password or backup binary is included in this document.

**Final classification:** configuration work reported, **not yet site-verified**.
