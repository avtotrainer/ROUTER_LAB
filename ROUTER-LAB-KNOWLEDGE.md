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
