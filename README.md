# Cortex XSOAR Playbooks — Brute Force & Vulnerability Remediation

Production-ready (and lab-friendly) XSOAR playbooks for:
1) **RDP Brute Force — Investigate & Contain**
2) **Nessus High Severity — Validate, Remediate & Verify**

These mirror my homelab workflows (Splunk, Sysmon, Suricata/Zeek, Nessus) and align to NIST 800-61 incident handling and MITRE ATT&CK mapping.

## Playbooks
- **PB-RDP-Bruteforce-Investigate-Contain.yml**
  - Pull events from Splunk, enrich source IPs, block malicious external IPs (Panorama example), isolate host (MDE), optionally lock AD accounts, notify SOC, open ticket.
- **PB-Nessus-High-Sev-Validate-Remediate-Verify.yml**
  - Get vuln details (Nessus), prioritize via KEV/EPSS, open change ticket, patch via Ivanti (optional), rescan to verify, close or escalate.

## Import (Cortex XSOAR)
1. **Settings → Playbooks → Import** and select a `.yml` from `/playbooks`.
2. Edit **Inputs** (thresholds, channels, toggles).
3. Map **Integrations** to your environment:
   - `SplunkPy` → your Splunk integration  
   - `Panorama` (or your firewall)  
   - `MicrosoftDefenderAdvancedThreatProtection` (MDE)  
   - `Active Directory Query v2`  
   - `ServiceNow v2` (or Jira/other ITSM)  
   - `SlackV3` (or Teams)
4. Test in a staging tenant before prod.

## Variables to Tune
- **Brute Force**: `failed_login_threshold`, `observation_window_minutes`, `auto_block_external_ip`, `edr_isolate_host`, `ad_lock_account`
- **Vuln Remediation**: `min_cvss`, `require_known_exploit`, `patch_via_ivanti`

## Notes
- Integration names in the YAML are examples—swap to whatever your stack uses.
- The KEV/EPSS lookup step assumes a wrapper integration or custom script; replace with your threat-intel source if needed.
- If you don’t have XSOAR, the YAML still documents **IR automation logic** you can port to other SOARs or scripts.

## Mapping
- **MITRE ATT&CK**: Credential Access (T1110), Discovery, Defense Evasion
- **NIST 800-61**: Detection → Analysis → Containment → Eradication → Recovery → Post-Incident

## License
MIT
