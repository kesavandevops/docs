# 📙 **L3 Linux Administrator Handbook**

*Advanced Linux System Administration & Production Engineering Guide*

---

# **1. Overview**

An **L3 Linux Administrator** is responsible for:

* Deep troubleshooting (kernel, network stack, filesystem corruption)
* High availability (HA) & clustering
* Automation and configuration management
* Performance tuning
* Security & compliance
* Production-grade infrastructure support
* Root cause analysis (RCA)
* Incident management & on-call support

This handbook builds on L2 by covering advanced tools, concepts, and enterprise-level skills.

---

# **2. Advanced System Troubleshooting**

## **2.1 Kernel Debugging**

Check kernel messages:

```bash
dmesg | less
journalctl -k
```

Check kernel version:

```bash
uname -r
```

Live kernel tuning:

```bash
sysctl -a
sysctl -w net.ipv4.ip_forward=1
```

Persist changes:

```
/etc/sysctl.conf
```

---

## **2.2 System Lockups / Freezes**

Analyze load:

```bash
uptime
top -H      # show threads
ps -eo pid,tid,pcpu,pmem,cmd --sort=-pcpu
```

Check OOM (Out-Of-Memory) events:

```bash
dmesg | grep -i "killed process"
```

---

## **2.3 Filesystem Corruption**

Force unmount:

```bash
umount -f /mnt/data
```

Run fsck:

```bash
fsck.ext4 /dev/sdb1
```

Repair XFS:

```bash
xfs_repair /dev/sdb1
```

---

# **3. Advanced Networking**

## **3.1 Deep Packet Analysis**

```bash
tcpdump -i eth0 -nn host 10.0.0.5
tcpdump -i eth0 port 443 -w capture.pcap
```

## **3.2 Routing & Policy-Based Routing**

```bash
ip rule add from 192.168.1.0/24 table 100
ip route add default via 192.168.1.1 table 100
```

## **3.3 Network Performance Tuning**

sysctl tuning:

```
net.core.somaxconn = 1024
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_max_syn_backlog = 2048
```

Apply:

```bash
sysctl -p
```

---

# **4. High Availability & Clustering**

## **4.1 Corosync + Pacemaker**

Check cluster status:

```bash
pcs status
pcs cluster status
```

Create resource:

```bash
pcs resource create vip ocf:heartbeat:IPaddr2 ip=10.10.10.50 cidr_netmask=24
```

---

## **4.2 Keepalived VRRP**

Example config `/etc/keepalived/keepalived.conf`:

```ini
vrrp_instance VI_1 {
  state MASTER
  interface eth0
  virtual_router_id 51
  priority 150
  virtual_ipaddress {
    192.168.0.10
  }
}
```

Restart service:

```bash
systemctl restart keepalived
```

---

# **5. Load Balancing**

## **5.1 Nginx Load Balancer**

```nginx
upstream backend {
  server 10.0.0.1;
  server 10.0.0.2;
}

server {
  listen 80;
  location / {
    proxy_pass http://backend;
  }
}
```

---

## **5.2 HAProxy**

```bash
frontend http_in
    bind *:80
    default_backend servers

backend servers
    server s1 10.0.0.1:80 check
    server s2 10.0.0.2:80 check
```

---

# **6. Virtualization & Containers (Advanced)**

## **6.1 KVM/QEMU**

List VMs:

```bash
virsh list --all
```

Start VM:

```bash
virsh start vm-name
```

---

## **6.2 Docker (Advanced)**

Inspect container networking:

```bash
docker network inspect bridge
```

Limit resources:

```bash
docker run -m 512m --cpus=1 nginx
```

---

## **6.3 Kubernetes Basics for L3**

```bash
kubectl get pods -A
kubectl describe node nodename
kubectl logs podname
```

---

# **7. Storage & Enterprise Filesystems**

## **7.1 NFS Advanced**

Check active mounts:

```bash
nfsstat -m
```

Server config:

```bash
/export 10.0.0.0/24(rw,sync,no_root_squash)
```

---

## **7.2 XFS Advanced Tools**

Check fragmentation:

```bash
xfs_db -c frag -r /dev/sda1
```

Grow XFS:

```bash
xfs_growfs /mountpoint
```

---

## **7.3 iSCSI**

Discover:

```bash
iscsiadm -m discovery -t sendtargets -p 10.0.0.10
```

Login:

```bash
iscsiadm -m node --login
```

---

# **8. Automation & Configuration Management**

## **8.1 Ansible (L3-Level Use Cases)**

### Run playbook:

```bash
ansible-playbook site.yml
```

### Use Ansible Vault:

```bash
ansible-vault encrypt secrets.yml
ansible-vault decrypt secrets.yml
```

### Advanced Inventory:

```
[webservers]
web1 ansible_host=10.0.0.1
web2 ansible_host=10.0.0.2
```

---

# **9. Logging, Auditing & Monitoring**

## **9.1 Centralized Logging**

* rsyslog
* ELK (Elastic, Logstash, Kibana)
* Graylog
* Loki + Promtail

Set remote logging:

```bash
*.* @10.0.0.10:514
```

---

## **9.2 Auditd**

Rules example:

```
-w /etc/passwd -p wa -k user_changes
-w /etc/ssh/sshd_config -p wa -k ssh_changes
```

View logs:

```bash
ausearch -k ssh_changes
aureport
```

---

# **10. Performance Tuning (Advanced)**

## **10.1 CPU**

```bash
tuned-adm profile latency-performance
```

## **10.2 Disk IO**

```bash
iostat -xz 1
```

## **10.3 Memory**

HugePages:

```bash
grep Huge /proc/meminfo
```

Set:

```
vm.nr_hugepages=128
```

---

# **11. Backup & Disaster Recovery**

## **11.1 Snapshot strategies**

* LVM snapshots
* ZFS snapshots
* VM snapshots
* Database backups

LVM snapshot example:

```bash
lvcreate -L 1G -s -n snap_data /dev/vg/lv_data
```

---

## **11.2 Disaster Recovery Checkpoints**

* DR site readiness
* Backup restore testing
* Replication checks
* Failover & failback procedures

---

# **12. Security (Advanced Hardening)**

## **12.1 SELinux advanced**

Find denials:

```bash
ausearch -m AVC
```

Generate fix:

```bash
audit2allow -M mypolicy
semodule -i mypolicy.pp
```

---

## **12.2 SSH Hardening**

Disable weak ciphers:

```
Ciphers aes256-ctr,aes192-ctr,aes128-ctr
MACs hmac-sha2-512,hmac-sha2-256
```

---

## **12.3 Compliance Tools**

* OpenSCAP
* Lynis
* CIS Benchmark

Scan:

```bash
lynis audit system
```

---

# **13. RCA (Root Cause Analysis) Framework**

1. **Incident summary**
2. **Timeline**
3. **Impact analysis**
4. **Logs/output collected**
5. **Root cause (technical detail)**
6. **Corrective actions**
7. **Preventive actions**
8. **Lessons learned**

---

# **14. L3-Level Interview / On-Call Scenarios**

### ❓ Application intermittently failing — how do you trace it?

* Check logs
* Increase verbosity
* Capture packets
* Monitor resource usage

### ❓ Filesystem corruption after crash — steps?

* Unmount
* fsck/XFS repair
* Check underlying disk with `smartctl`

### ❓ HA cluster failover not happening — what to check?

* Resource constraints
* Fencing
* Corosync connectivity
* Quorum

### ❓ Server boot stuck — how to handle?

* Switch to rescue mode
* Check initramfs
* Rebuild GRUB
* Analyze `journalctl -xb`
