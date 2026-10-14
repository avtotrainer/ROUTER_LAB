# Router Lab — Knowledge Base

## 1. Purpose

Document tested procedures for configuring, recovering, and maintaining routers.

## 2. Workstation

- Operating system: NixOS
- Network management: NetworkManager
- Primary configuration tool: `nmcli`
- Preserve Wi-Fi connectivity when configuring routers through Ethernet.

## 3. Supported Router Firmware

- DD-WRT
- OpenWrt
- Stock firmware

## 4. Network Configuration

Maintain separate NetworkManager profiles for automatic DHCP and static router configuration.

Avoid changing the default route unnecessarily.

## 5. Device Records

For each router, record:

- Manufacturer and model
- Hardware revision
- Firmware version
- Management IP address
- Current operating mode
- Backup location
- Tested configuration procedures

## 6. Troubleshooting

Record each problem using:

- Symptoms
- Diagnosis
- Commands executed
- Verified solution
- Recovery procedure

## 7. Safety

- Back up existing configurations before making changes.
- Verify the exact hardware revision before flashing firmware.
- Never store passwords, tokens, or private keys in this repository.
- Clearly distinguish tested solutions from proposed procedures.

## 8. Mandatory Post-Configuration Validation

Before marking any router configuration **Complete**, perform and document the following workflow:

1. Confirm exact router model, hardware revision, and installed firmware build directly on the device.
2. Test LAN management connectivity, including the management IP/subnet and expected reachability from the NixOS workstation.
3. Test Wi-Fi operation when applicable: SSID visibility, client association, and connectivity through the intended network path.
4. Verify bridge/access-point membership and forwarding when applicable; confirm expected WAN, LAN, and wireless interface behavior.
5. Verify DHCP behavior matches the intended topology (enabled and serving correct settings, or disabled with no conflicting service). Check DNS and gateway behavior as appropriate.
6. Create a fresh configuration backup, store it in the **private** Google Drive `ROUTER_LAB/02_BACKUPS/<device-id>/` folder, and confirm its filename and size. Never upload backup binaries or credentials to public GitHub.
7. Archive the applicable firmware images in Google Drive `ROUTER_LAB/01_FIRMWARE/<device-id-or-model>/`; record source, version/build if verified, SHA-256 checksums, and links in the GitHub device record. Do not flash solely because an image has been archived.
8. Update the device record under `devices/` with dated test results, evidence, backup/firmware references, remaining risks, and final status.

Use **Pass**, **Fail**, or **Not applicable** for each check, and explicitly distinguish **Not tested** from **Pass**. A router with untested required checks must remain **Pending validation**, not **Complete**. Restoration testing is optional and potentially disruptive; document whether it was performed, and require explicit authorization before restoring a backup.
