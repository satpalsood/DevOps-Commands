# List of Common Services, Applications, and Their Default Ports

## Well-Known Ports (0-1023)

```

| Port | Protocol | Service/Application                         |
|------|----------|---------------------------------------------|
| 20   | TCP      | FTP (Data Transfer)                        |
| 21   | TCP      | FTP (Control)                              |
| 22   | TCP      | SSH (Secure Shell)                         |
| 23   | TCP      | Telnet                                     |
| 25   | TCP      | SMTP (Simple Mail Transfer Protocol)       |
| 53   | UDP/TCP  | DNS (Domain Name System)                  |
| 67   | UDP      | DHCP (Server)                              |
| 68   | UDP      | DHCP (Client)                              |
| 69   | UDP      | TFTP (Trivial File Transfer Protocol)      |
| 80   | TCP      | HTTP (Web Traffic)                        |
| 88   | TCP      | Kerberos Authentication                   |
| 110  | TCP      | POP3 (Post Office Protocol)               |
| 119  | TCP      | NNTP (Network News Transfer Protocol)     |
| 123  | UDP      | NTP (Network Time Protocol)               |
| 135  | TCP      | RPC (Remote Procedure Call)               |
| 137  | UDP      | NetBIOS Name Service                      |
| 138  | UDP      | NetBIOS Datagram Service                  |
| 139  | TCP      | NetBIOS Session Service                   |
| 143  | TCP      | IMAP (Internet Message Access Protocol)   |
| 161  | UDP      | SNMP (Simple Network Management Protocol) |
| 162  | UDP      | SNMP Trap                                 |
| 179  | TCP      | BGP (Border Gateway Protocol)             |
| 389  | TCP/UDP  | LDAP (Lightweight Directory Access Protocol) |
| 443  | TCP      | HTTPS (Secure Web Traffic)               |
| 445  | TCP      | SMB (Server Message Block)               |
| 465  | TCP      | SMTPS (Secure SMTP)                      |
| 514  | UDP      | Syslog                                   |
| 636  | TCP      | LDAPS (Secure LDAP)                      |
| 989  | TCP      | FTPS (Data)                              |
| 990  | TCP      | FTPS (Control)                           |

---
```

## Registered Ports (1024-49151)
```
| Port  | Protocol | Service/Application          |
|-------|----------|------------------------------|
| 1433  | TCP      | Microsoft SQL Server        |
| 1521  | TCP      | Oracle Database            |
| 2049  | TCP/UDP  | NFS (Network File System)  |
| 2181  | TCP      | Apache Zookeeper           |
| 2379  | TCP      | etcd (Kubernetes)          |
| 3306  | TCP      | MySQL Database             |
| 3389  | TCP      | RDP (Remote Desktop Protocol) |
| 4000  | TCP      | Percona XtraDB Cluster     |
| 5432  | TCP      | PostgreSQL Database        |
| 5672  | TCP      | RabbitMQ (AMQP)            |
| 5900  | TCP      | VNC (Virtual Network Computing) |
| 6379  | TCP      | Redis                      |
| 6443  | TCP      | Kubernetes API Server      |
| 6665-6669 | TCP  | IRC (Internet Relay Chat)  |
| 8080  | TCP      | HTTP Alternate (Tomcat, Web Servers) |
| 8081  | TCP      | Nexus Repository Manager   |
| 9090  | TCP      | Prometheus Monitoring      |
| 9093  | TCP      | Alertmanager (Prometheus)  |
| 9200  | TCP      | Elasticsearch              |
| 9418  | TCP      | Git Protocol               |
| 10250 | TCP      | Kubelet API                |
| 10255 | TCP      | Kubernetes Read-Only API   |

---
```

## Dynamic and Private Ports (49152-65535)
These ports are assigned dynamically for client applications and are not fixed. Examples include:
- **Ephemeral Ports** used for client-server communication
- **Docker Containers** typically use ports in this range
- **Kubernetes Services** expose NodePort services in the range `30000-32767`

