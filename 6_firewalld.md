
# FIREWALLD — QUICK MASTER GUIDE

Firewalld is zone-based firewall.

Service:  
```
systemctl enable --now firewalld
```

---

# 🔥 1. Zones (Very Important)

List zones:
```
firewall-cmd --get-zones
```

Active zone:
```
firewall-cmd --get-active-zones
```

Set default zone:
```
firewall-cmd --set-default-zone=public
```

---

# 🔥 2. Allow / Remove Services

### Allow SSH:
```
firewall-cmd --permanent --add-service=ssh
firewall-cmd --reload
```

### Allow HTTP:
```
firewall-cmd --permanent --add-service=http
firewall-cmd --reload
```

### Remove service:
```
firewall-cmd --permanent --remove-service=http
firewall-cmd --reload
```

---

# 🔥 3. Allow Ports

```
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --reload
```

Remove:
```
firewall-cmd --permanent --remove-port=8080/tcp
firewall-cmd --reload
```

---

# 🔥 4. Rich Rules (Advanced)

Allow only 1 IP:
```
firewall-cmd --permanent --add-rich-rule="rule family=ipv4 source address=1.2.3.4 accept"
```

Block IP:
```
firewall-cmd --permanent --add-rich-rule="rule family=ipv4 source address=1.2.3.4 drop"
```

---

# 🔥 5. Zones per Interface

```
firewall-cmd --permanent --zone=public --add-interface=eth0
firewall-cmd --reload
```

---

# 🔥 6. See Active Rules Dump

```
firewall-cmd --list-all
```

---

# 🔥 7. Config Files (Reference)

Permanent configs live here:
```
/etc/firewalld/zones/*.xml
```

Do NOT edit unless required. Use `firewall-cmd` instead.

Runtime configs live here:
```
/run/firewalld/*
```

---

