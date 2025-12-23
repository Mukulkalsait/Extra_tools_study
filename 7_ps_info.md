
# PS — PROCESS LISTING MASTER GUIDE

---

## 🔥 Most Common Commands

### List all processes
```
ps aux
```
- `a` → all users  
- `u` → show username + CPU + MEM  
- `x` → include non-TTY processes  

---

### Sorted by CPU
```
ps aux --sort=-%cpu
```

### Sorted by Memory
```
ps aux --sort=-%mem
```

---

### Tree view
```
ps axjf
```
OR
```
ps -ejH
```

---

### Show only processes of a user
```
ps -u examuser-ubuntu
```

---

### Show PID, PPID, TIME, COMMAND only
```
ps -eo pid,ppid,cmd,etime
```

---

### Show systemd tree (very useful in your lab)
```
ps -e --forest
```

---

# 🔥 Fields Explained

| Field | Meaning |
|------|---------|
| PID | Process ID |
| PPID | Parent PID |
| TTY | Terminal |
| STAT | Process state (R,S,D,T,Z,…) |
| %CPU | CPU usage |
| %MEM | Memory usage |
| COMMAND | Command executed |

---

