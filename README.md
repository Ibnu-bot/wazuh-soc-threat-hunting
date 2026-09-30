# wazuh-soc-threat-hunting
🛡️ SOC Investigation &amp; Threat Hunting playground using Wazuh SIEM. Analyzed SSH brute force, anomalous geographic logins, and off-hours account creations with 5W1H methodology.


<details>
<summary><b>🔍 Quest 1: SSH Brute Force Analysis (Klik untuk melihat detail)</b></summary>

### 📊 Analisis 5W + 1H
* **What**: Percobaan SSH Password Guessing yang ditolak oleh sistem.
* **Who**: 
  * **Attacker**: `45.156.87.146` (Germany)
  * **Target**: `root`
* **When**: 17 September 2026, 05:48:05 – 05:48:07 WIB
* **Where**: Agent `jida-wazuh-manager`
* **Why**: IP asal melakukan percobaan berulang terhadap akun akses paling tinggi (`root`).
* **How**: Mengirimkan request autentikasi SSH (Port 58788) secara berturut-turut.

### 🛡️ Rekomendasi Mitigasi
1. Nonaktifkan login root langsung via SSH (`PermitRootLogin no`).
2. Terapkan Active Response di Wazuh untuk memblokir IP penyerang secara otomatis via `iptables` setelah $N$ kali kegagalan login.
3. Wajibkan penggunaan SSH Key-Based Authentication.

</details>
