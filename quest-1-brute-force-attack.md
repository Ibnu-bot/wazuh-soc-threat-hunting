# 🔍 Quest 1: SSH Brute Force Analysis

## 📌 Deskripsi Kasus
Terdeteksi beberapa upaya percobaan masuk (*authentication failure*) ke akun administrator (`root`) yang dikirimkan secara berurutan oleh IP asing.

---

## 📑 Raw Log Evidence (JSON Sample)
```json
{
  "timestamp": "Sep 17, 2026 @ 05:48:07.347",
  "agent": {
    "name": "jida-wazuh-manager",
    "id": "000"
  },
  "data": {
    "srcip": "45.156.87.146",
    "dstuser": "root",
    "srcport": 58788
  },
  "rule": {
    "id": "5760",
    "description": "sshd: authentication failed.",
    "level": 5,
    "mitre": {
      "id": ["T1110.001", "T1021.004"],
      "tactic": ["Credential Access", "Lateral Movement"]
    }
  },
  "GeoLocation": {
    "country_name": "Germany"
  }
} 
