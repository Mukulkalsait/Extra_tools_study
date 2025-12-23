# SSH CONFIG (Client)
This file configures the *SSH client* — how **you** connect to servers.

Located at:
- Global: `/etc/ssh/ssh_config`
- User: `~/.ssh/config` ← **Most common**

---

## 🔥 Basic Useful Options

### Host *
```
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
```
- Prevents SSH session freezing/disconnections.

---

### Custom Host Shortcut

```
Host rocky
    HostName 127.0.0.1
    Port 2202
    User examuser-rocky
```

Usage:

```
ssh rocky
```

---

### IdentityFile
```
IdentityFile ~/.ssh/id_rsa
```
- Specifies which key to use for this host.

---

### StrictHostKeyChecking no
```
StrictHostKeyChecking no
```
- Auto-accept host key on first connection.
- Useful in labs/containers.

---

### UserKnownHostsFile /dev/null
```
UserKnownHostsFile /dev/null
```
- Avoids updating `~/.ssh/known_hosts`.
- Good for frequently rebuilding containers.

---

## 🔥 Full Useful Example

```
Host rocky
    HostName 127.0.0.1
    Port 2202
    User examuser-rocky
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

---

