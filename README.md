# Windows Server RDS Learning Path 43 — Configure Windows Firewall for RDS

**Level:** Advanced · **Module:** 43/70

## Goal
Restrict RDS-related Windows Firewall rules to required profiles, ports and source networks.

## Setup
1. Export the current firewall policy.
2. Inventory rules for RDP, RD Web, Gateway, Broker and management.
3. For internal Session Hosts, scope RDP to approved management or Gateway networks.
4. Keep Public-profile exposure disabled unless explicitly required.
5. Test one allowed and one blocked source path.
6. Document perimeter controls separately from host Firewall.

```powershell
netsh.exe advfirewall export "$env:TEMP\RDS-firewall-before.wfw"
Get-NetFirewallRule -DisplayGroup 'Remote Desktop' |
 Get-NetFirewallPortFilter
Get-NetFirewallRule -Enabled True | Where-Object DisplayName -match 'Remote|RD|IIS'
Test-NetConnection RDS01 -Port 3389
```

## Evidence
Store the firewall export, rule/profile/source scope, allowed test, blocked test and relevant Firewall events under `evidence/`.

## Troubleshooting
When a path fails, test DNS, routing and port reachability before changing rules. Avoid disabling Firewall as a diagnostic shortcut.

## Security
Open only required ports from required sources. External users should later use RD Gateway over HTTPS.

## Rollback
Import the saved firewall policy from console access if the test rules cause a lockout.

## Next
`Windows-Server-RDS-Learning-Path-44-Deploy-Windows-LAPS-to-RDS01`
