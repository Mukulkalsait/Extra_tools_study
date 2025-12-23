
# SSHD CONFIG (Server)
This file controls how the SSH server behaves on the machine.

---

## 🔥 Common Important Options (Explained)

### Port 22
```
Port 22
```
- SSH server listens on port 22.
- You can change it to 2222, 2201, 5022 for security or testing.

---

### PermitRootLogin no
```
PermitRootLogin no
```
- Disallows SSH login *as root*.
- You MUST login as normal user + `sudo`.
- **Exam-safe**, prevents accidental root password usage.

---

### PasswordAuthentication yes
```
PasswordAuthentication yes
```
- Allows users to login using password.
- Required for RHCSA-style labs where SSH key may not be provided.

---

### ChallengeResponseAuthentication no
```
ChallengeResponseAuthentication no
```
- Disables challenge-response mechanisms (OTP, MFA).
- Not required in basic servers.

---

### UsePAM yes
```
UsePAM yes
```
- Enables PAM (Pluggable Authentication Modules).
- Required for:
  - password policies  
  - login limits  
  - account lockout  
  - `/etc/security/*` rules  

---

### X11Forwarding no
```
X11Forwarding no
```
- Disables GUI forwarding over SSH.
- Save resources, increase security.

---

### AllowTcpForwarding yes
```
AllowTcpForwarding yes
```
- Enables port forwarding/tunneling:
  - `ssh -L 8080:127.0.0.1:80 user@host`
- Required for:
  - git over SSH  
  - database tunnels  
  - devops port-forward experiments  

---

## 🔥 Full Minimal Secure Example

```
Port 22
PermitRootLogin no
PasswordAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding no
AllowTcpForwarding yes
Subsystem sftp /usr/lib/ssh/sftp-server
```

---


