# Router Device Record — Template

> Copy this file to `devices/<manufacturer>-<model>-<hardware-revision>.md` for each physical router. Replace placeholders; use `Unknown` for unverified details. Never store passwords, private keys, Wi-Fi passphrases, or backup secrets in GitHub.

## Identity

| Field | Value |
| --- | --- |
| Device ID / asset tag | TODO |
| Manufacturer | TODO |
| Model | TODO |
| Hardware revision | Unknown |
| Serial number | Not stored in public repository |
| Location / role | TODO (avoid sensitive location details) |
| Inventory status | In service / Lab / Spare / Retired |
| Last verified (YYYY-MM-DD) | TODO |

## Hardware and firmware

| Field | Value |
| --- | --- |
| CPU / RAM / flash (if known) | Unknown |
| Firmware family | Stock / DD-WRT / OpenWrt / Other |
| Firmware version / build | Unknown |
| Firmware source URL | TODO |
| Firmware image checksum (SHA-256) | Unknown |
| Supported recovery method | Unknown |
| Recovery documentation | TODO |

## Network configuration

| Field | Value |
| --- | --- |
| Operating mode | Router / Access point / Bridge / Repeater / Other |
| Management IPv4 / subnet | Unknown |
| Management access protocol | HTTP / HTTPS / SSH / Other |
| LAN subnet | Unknown |
| DHCP server | Enabled / Disabled / Unknown |
| WAN uplink type | Unknown |
| VLANs (non-sensitive summary) | None / Unknown |
| Wi-Fi SSIDs (optional, non-sensitive) | Not recorded |
| Connection tested from NixOS | No |
| Last connectivity test | Not tested |

## Backup and recovery

| Field | Value |
| --- | --- |
| Backup date (YYYY-MM-DD) | None |
| Google Drive backup path | `ROUTER_LAB/02_BACKUPS/<device-id>/` |
| Backup filename / version | None |
| Restore procedure reference | TODO |
| Restore tested | No |

> Keep actual backups in the designated private Google Drive folder. Check whether backups contain secrets before sharing or uploading them.

## Change log

| Date (YYYY-MM-DD) | Change | Result | Reference |
| --- | --- | --- | --- |
| TODO | Initial inventory entry | Pending verification | — |

## Issues and diagnostics

- **Symptoms:** None recorded
- **Observed evidence / commands:** None recorded
- **Diagnosis:** Not assessed
- **Resolution:** Not tested
- **Rollback plan:** Not documented

## Verification checklist

- [ ] Exact model and hardware revision confirmed
- [ ] Firmware version confirmed from device
- [ ] Management IP and subnet verified
- [ ] Network route and connectivity tested
- [ ] Backup created and storage path recorded
- [ ] Recovery method reviewed
- [ ] No credentials or sensitive identifiers committed

## References

- Vendor documentation: TODO
- Firmware release notes: TODO
- Related Router Lab documentation: TODO
