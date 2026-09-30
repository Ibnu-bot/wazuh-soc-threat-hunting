# 🔍 Quest 2: Anomalous Successful Login Analysis

## 📌 Deskripsi Kasus
Terdeteksi keberhasilan autentikasi masuk (*Accepted password*) ke akun administrator (`root`) via SSH dari lokasi geolokasi tidak biasa (Ashburn, Amerika Serikat) pada server `jida-wazuh-manager`.

---

## 📑 Raw Log Evidence (JSON Sample)

```json
{
  "timestamp": "Sep 21, 2026 @ 23:37:37.352",
  "agent": {
    "id": "000",
    "name": "jida-wazuh-manager"
  },
  "data": {
    "srcip": "52.200.44.199",
    "dstuser": "root",
    "srcport": 60328
  },
  "rule": {
    "id": "5715",
    "description": "sshd: authentication success.",
    "level": 3,
    "groups": ["syslog", "sshd", "authentication_success"],
    "mitre": {
      "id": ["T1078", "T1021"],
      "tactic": ["Initial Access", "Persistence", "Privilege Escalation", "Lateral Movement"],
      "technique": ["Valid Accounts", "Remote Services"]
    }
  },
  "GeoLocation": {
    "country_name": "United States",
    "city_name": "Ashburn",
    "region_name": "Virginia"
  },
  "full_log": "Sep 21 16:37:36 jida-wazuh-manager sshd[818940]: Accepted password for root from 52.200.44.199 port 60328 ssh2"
}
