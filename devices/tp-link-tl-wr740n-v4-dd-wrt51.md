# TP-Link TL-WR740N v4 — DD-WRT51

## Identity

| Field | Value |
| --- | --- |
| Manufacturer | TP-Link |
| Model | TL-WR740N |
| Hardware revision | v4 |
| Device name | DD-WRT51 |
| Inventory status | Previously configured; device not currently on hand |
| Firmware family | DD-WRT |
| Exact firmware build | Not recorded |

## Verified configuration from previous Router Lab session

| Field | Value |
| --- | --- |
| Role | LAN-to-Wi-Fi bridge / access point |
| Management IPv4 | 10.107.143.51/24 |
| Gateway | 10.107.143.1 |
| DNS | 10.107.143.1 |
| LAN addressing | Static |
| WAN | Disabled |
| DHCP server | Disabled |
| dnsmasq | Disabled |
| Bridge | br0 with eth0, eth1, wlan0 |
| Wi-Fi SSID | MES |
| Wi-Fi mode | AP / mixed |
| Security | WPA2-PSK / AES (passphrase intentionally omitted) |
| Wi-Fi channel | 1 (2412 MHz) |
| Channel width | 20 MHz |

## Troubleshooting history

DD-WRT generated invalid wireless channel/frequency values (`channel=-225`, `frequency=1`). A `hostapd` configuration adjustment was made and startup persistence was configured via `rc_startup`. Reboot validation previously confirmed that the AP/hostapd and bridge operated, with DHCP and dnsmasq disabled.

**Important:** The exact `hostapd` and `rc_startup` command contents are not recorded in this inventory entry; do not reconstruct or execute them from memory. Retrieve the original verified commands before reapplying to a device.

## Backup and recovery

- Google Drive backup destination: `ROUTER_LAB/02_BACKUPS/DD-WRT51/`
- Backup existence: Not verified
- Restore procedure: Not recorded
- Recovery test: Not recorded

## Follow-up checks when the device is available

- [ ] Read exact DD-WRT firmware build
- [ ] Confirm management IP and bridge interfaces
- [ ] Confirm Wi-Fi channel and association
- [ ] Confirm DHCP/dnsmasq remain disabled
- [ ] Capture current `hostapd` and `rc_startup` configuration safely (exclude secrets)
- [ ] Save and verify a configuration backup in Google Drive

## Notes

This entry records previously reported configuration and validation. The router was not physically available during the current NixOS NetworkManager checks, so no new live device verification was performed.
