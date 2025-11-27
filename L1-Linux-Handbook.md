# L1-Linux-Administrator-Handbook
A guide that covers essential commands, system management tasks, and troubleshooting techniques

---

## 🐧 **L1 Linux Administrator Handbook**

### 1. **System Information & Management**

* **Check system info**:

  ```bash
  uname -a
  ```
* **Check disk usage**:

  ```bash
  df -h
  ```
* **Check memory usage**:

  ```bash
  free -h
  ```
* **View system logs**:

  ```bash
  tail -f /var/log/syslog
  ```

### 2. **User & Group Management**

* **Add a user**:

  ```bash
  sudo useradd username
  ```
* **Set user password**:

  ```bash
  sudo passwd username
  ```
* **Add user to a group**:

  ```bash
  sudo usermod -aG groupname username
  ```
* **Create a group**:

  ```bash
  sudo groupadd groupname
  ```
* **Delete a user**:

  ```bash
  sudo userdel username
  ```
* **Delete a group**:

  ```bash
  sudo groupdel groupname
  ```

### 3. **File & Directory Management**

* **Create a directory**:

  ```bash
  mkdir directoryname
  ```
* **Create a file**:

  ```bash
  touch filename
  ```
* **Copy files**:

  ```bash
  cp source destination
  ```
* **Move or rename files**:

  ```bash
  mv source destination
  ```
* **Remove files**:

  ```bash
  rm filename
  ```
* **Remove directories**:

  ```bash
  rm -r directoryname
  ```

### 4. **File Permissions & Ownership**

* **Change file permissions**:

  ```bash
  chmod permissions filename
  ```
* **Change file owner**:

  ```bash
  chown owner:group filename
  ```
* **View file permissions**:

  ```bash
  ls -l filename
  ```

### 5. **Package Management**

* **Debian/Ubuntu**:

  * **Update package list**:

    ```bash
    sudo apt update
    ```
  * **Upgrade packages**:

    ```bash
    sudo apt upgrade
    ```
  * **Install a package**:

    ```bash
    sudo apt install packagename
    ```
  * **Remove a package**:

    ```bash
    sudo apt remove packagename
    ```

* **Red Hat/CentOS**:

  * **Update package list**:

    ```bash
    sudo yum check-update
    ```
  * **Upgrade packages**:

    ```bash
    sudo yum update
    ```
  * **Install a package**:

    ```bash
    sudo yum install packagename
    ```
  * **Remove a package**:

    ```bash
    sudo yum remove packagename
    ```

### 6. **Networking**

* **Check network interfaces**:

  ```bash
  ip a
  ```
* **Check network connectivity**:

  ```bash
  ping domain_or_ip
  ```
* **View routing table**:

  ```bash
  ip route
  ```
* **Check open ports**:

  ```bash
  sudo netstat -tuln
  ```

### 7. **Process Management**

* **View running processes**:

  ```bash
  ps aux
  ```
* **View processes by user**:

  ```bash
  ps -u username
  ```
* **Kill a process**:

  ```bash
  sudo kill process_id
  ```
* **Kill a process by name**:

  ```bash
  sudo pkill processname
  ```

### 8. **System Services**

* **Start a service**:

  ```bash
  sudo systemctl start service_name
  ```
* **Stop a service**:

  ```bash
  sudo systemctl stop service_name
  ```
* **Enable a service to start on boot**:

  ```bash
  sudo systemctl enable service_name
  ```
* **Disable a service from starting on boot**:

  ```bash
  sudo systemctl disable service_name
  ```
* **Check the status of a service**:

  ```bash
  sudo systemctl status service_name
  ```

### 9. **Backup & Restore**

* **Create a backup using tar**:

  ```bash
  tar -cvf backup.tar /path/to/directory
  ```
* **Extract a backup**:

  ```bash
  tar -xvf backup.tar
  ```

### 10. **Security & Firewall**

* **Check firewall status**:

  ```bash
  sudo ufw status
  ```
* **Allow a port through the firewall**:

  ```bash
  sudo ufw allow port_number
  ```
* **Deny a port through the firewall**:

  ```bash
  sudo ufw deny port_number
  ```
* **Enable the firewall**:

  ```bash
  sudo ufw enable
  ```
* **Disable the firewall**:

  ```bash
  sudo ufw disable
  ```

---
