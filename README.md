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


<details>
<summary><b>🔍 Quest 2: Anomalous Successful Login Analysis</b></summary>

## 📌 Deskripsi Kasus
Terdeteksi keberhasilan autentikasi masuk (*Accepted password*) ke akun administrator (`root`) via SSH dari lokasi geolokasi tidak biasa (Ashburn, Amerika Serikat) pada server `jida-wazuh-manager`.

📊 Analisis 5W + 1
HWHAT: Autentikasi SSH berhasil (sshd: authentication success) menggunakan kata sandi (Accepted password).
WHO:Attacker/Actor: IP 52.200.44.199 (AWS Data Center - Ashburn, United States)Target Account: root
WHEN: 21 September 2026 @ 23:37:37 WIB (Event System Time: Sep 21 16:37:36 UTC)
WHERE: Agent jida-wazuh-manager (agent.id: 000)
WHY: Akun paling krusial (root) berhasil diakses dari IP publik AS (Ashburn, VA) yang tidak terdaftar dalam jangkauan IP operasional tim internal, mengindikasikan adanya pencurian kredensial (Compromised Credentials).
HOW: Aktor menggunakan kredensial root yang valid via layanan SSH (Port 60328) untuk mendapatkan akses langsung ke sistem.

🛡️️ Rekomendasi Mitigasi
Isolasi Sesi & Reset Kredensial:
- Segera putuskan sesi SSH yang sedang aktif dari IP 52.200.44.199 (pkill -9 -u root atau menggunakan ss).
- Ganti kata sandi root secara mendesak.
  
Penguatan Otentikasi (Hardening):
- Matikan fitur login root langsung melalui file /etc/ssh/sshd_config (PermitRootLogin no).
- Wajibkan penggunaan Multi-Factor Authentication (MFA) atau SSH Key-Based Authentication dengan passphrase.
Pembatasan Jaringan (Network Restriction):
Batasi akses port SSH (22) hanya dari segmen IP internal / VPN resmi menggunakan firewall (ufw / iptables).

</details>

<details>
<summary><b>🔍 Quest 3: Off-Hours Account & Group Creation Analysis</b></summary>
📌 Deskripsi Kasus
Terdeteksi aktivitas pembuatan grup baru (maintenance-account) dan dilanjutkan dengan pembuatan akun pengguna baru (maintenance-account) pada sistem Linux di luar jam kerja/operasional normal (pukul 23:09 WIB) tanpa adanya Change Request resmi.

📊 Analisis 5W + 1H
- What: Eksekusi penambahan grup sistem baru (groupadd) dan akun pengguna baru (useradd) yang memicu alert Wazuh Level 8.
- Who:Target/Akun Baru: maintenance-account (UID: 1003, GID: 1003)Sesi Eksekusi: Sesi terminal interaktif /dev/pts/1
- When: 17 September 2026, 23:09:38 WIB (Event Log Time: Sep 17 16:09:37 UTC)
- Where: Agent ip-172-31-15-17 (agent.id: 002 | IP Internal: 172.31.15.17)
- Why: Penambahan akun dilakukan di malam hari (pukul 23:09 WIB) di luar jam kerja operasional resmi tanpa dokumentasi tiket perubahan yang terdaftar, mengindikasikan upaya pembentukan jalur Persistence (akses bertahan) oleh penyerang.
- How: Penyerang memanfaatkan akses terminal interaktif (/dev/pts/1) dengan hak akses administrator/root untuk mengeksekusi biner groupadd diikuti biner useradd.

🛡️ Rekomendasi Mitigasi
1. Konfirmasi & Verifikasi Otentisitas:
   Periksa dengan tim internal IT Operations apakah ada jadwal pemeliharaan khusus pada jam 23:09
   WIB tersebut.

2. Isolasi / Hapus Akun Mencurigakan:
   Jika aktivitas ini tidak terotorisasi, segera kunci dan hapus akun beserta direktori home-nya
   menggunakan perintah:


