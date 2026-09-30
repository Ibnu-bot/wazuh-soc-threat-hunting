## 📑 Raw Log Evidence (JSON Samples)

### Log 1: New Group Creation (Rule ID 5901)
```json
{
  "timestamp": "Sep 17, 2026 @ 23:09:38.873",
  "agent": {
    "id": "002",
    "name": "ip-172-31-15-17"
  },
  "data": {
    "dstuser": "maintenance-account",
    "gid": 1003
  },
  "rule": {
    "id": "5901",
    "description": "New group added to the system.",
    "level": 8
  },
  "full_log": "Sep 17 16:09:37 jida-wazuh-testing groupadd[5818]: new group: name=maintenance-account, GID=1003"
}

### Log 2: New User Creation (Rule ID 5902)
```json

{
  "timestamp": "Sep 17, 2026 @ 23:09:38.873",
  "agent": {
    "id": "002",
    "name": "ip-172-31-15-17"
  },
  "data": {
    "dstuser": "maintenance-account",
    "uid": 1003,
    "gid": 1003,
    "home": "/home/maintenance-account",
    "shell": "/bin/bash"
  },
  "rule": {
    "id": "5902",
    "description": "New user added to the system.",
    "level": 8
  },
  "full_log": "Sep 17 16:09:37 jida-wazuh-testing useradd[5825]: new user: name=maintenance-account, UID=1003, GID=1003, home=/home/maintenance-account, shell=/bin/bash, from=/dev/pts/1"
}
