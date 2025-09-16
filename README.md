# shotcut Installation Guide

shotcut is a free and open-source video editor. Shotcut provides cross-platform video editor

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Supported Operating Systems](#supported-operating-systems)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Service Management](#service-management)
6. [Troubleshooting](#troubleshooting)
7. [Security Considerations](#security-considerations)
8. [Performance Tuning](#performance-tuning)
9. [Backup and Restore](#backup-and-restore)
10. [System Requirements](#system-requirements)
11. [Support](#support)
12. [Contributing](#contributing)
13. [License](#license)
14. [Acknowledgments](#acknowledgments)
15. [Version History](#version-history)
16. [Appendices](#appendices)

## 1. Prerequisites

- **Hardware Requirements**:
  - CPU: 4+ cores
  - RAM: 8GB minimum
  - Storage: 50GB for projects
  - Network: GUI application
- **Operating System**: 
  - Linux: Any modern distribution (RHEL, Debian, Ubuntu, CentOS, Fedora, Arch, Alpine, openSUSE)
  - macOS: 10.14+ (Mojave or newer)
  - Windows: Windows Server 2016+ or Windows 10
  - FreeBSD: 11.0+
- **Network Requirements**:
  - Port N/A (default shotcut port)
  - None
- **Dependencies**:
  - See official documentation for specific requirements
- **System Access**: root or sudo privileges required


## 2. Supported Operating Systems

This guide supports installation on:
- RHEL 8/9 and derivatives (CentOS Stream, Rocky Linux, AlmaLinux)
- Debian 11/12
- Ubuntu 20.04/22.04/24.04 LTS
- Arch Linux (rolling release)
- Alpine Linux 3.18+
- openSUSE Leap 15.5+ / Tumbleweed
- SUSE Linux Enterprise Server (SLES) 15+
- macOS 12+ (Monterey and later) 
- FreeBSD 13+
- Windows 10/11/Server 2019+ (where applicable)

## 3. Installation

### RHEL/CentOS/Rocky Linux/AlmaLinux

```bash
# Install EPEL repository if needed
sudo dnf install -y epel-release

# Install shotcut
sudo dnf install -y shotcut

# Enable and start service
sudo systemctl enable --now shotcut

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Verify installation
shotcut --version
```

### Debian/Ubuntu

```bash
# Update package index
sudo apt update

# Install shotcut
sudo apt install -y shotcut

# Enable and start service
sudo systemctl enable --now shotcut

# Configure firewall
sudo ufw allow N/A

# Verify installation
shotcut --version
```

### Arch Linux

```bash
# Install shotcut
sudo pacman -S shotcut

# Enable and start service
sudo systemctl enable --now shotcut

# Verify installation
shotcut --version
```

### Alpine Linux

```bash
# Install shotcut
apk add --no-cache shotcut

# Enable and start service
rc-update add shotcut default
rc-service shotcut start

# Verify installation
shotcut --version
```

### openSUSE/SLES

```bash
# Install shotcut
sudo zypper install -y shotcut

# Enable and start service
sudo systemctl enable --now shotcut

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Verify installation
shotcut --version
```

### macOS

```bash
# Using Homebrew
brew install shotcut

# Start service
brew services start shotcut

# Verify installation
shotcut --version
```

### FreeBSD

```bash
# Using pkg
pkg install shotcut

# Enable in rc.conf
echo 'shotcut_enable="YES"' >> /etc/rc.conf

# Start service
service shotcut start

# Verify installation
shotcut --version
```

### Windows

```bash
# Using Chocolatey
choco install shotcut

# Or using Scoop
scoop install shotcut

# Verify installation
shotcut --version
```

## Initial Configuration

### Basic Configuration

```bash
# Create configuration directory
sudo mkdir -p /etc/shotcut

# Set up basic configuration
# See official documentation for detailed configuration options

# Test configuration
shotcut --version
```

## 5. Service Management

### systemd (RHEL, Debian, Ubuntu, Arch, openSUSE)

```bash
# Enable service
sudo systemctl enable shotcut

# Start service
sudo systemctl start shotcut

# Stop service
sudo systemctl stop shotcut

# Restart service
sudo systemctl restart shotcut

# Check status
sudo systemctl status shotcut

# View logs
sudo journalctl -u shotcut -f
```

### OpenRC (Alpine Linux)

```bash
# Enable service
rc-update add shotcut default

# Start service
rc-service shotcut start

# Stop service
rc-service shotcut stop

# Restart service
rc-service shotcut restart

# Check status
rc-service shotcut status
```

### rc.d (FreeBSD)

```bash
# Enable in /etc/rc.conf
echo 'shotcut_enable="YES"' >> /etc/rc.conf

# Start service
service shotcut start

# Stop service
service shotcut stop

# Restart service
service shotcut restart

# Check status
service shotcut status
```

### launchd (macOS)

```bash
# Using Homebrew services
brew services start shotcut
brew services stop shotcut
brew services restart shotcut

# Check status
brew services list | grep shotcut
```

### Windows Service Manager

```powershell
# Start service
net start shotcut

# Stop service
net stop shotcut

# Using PowerShell
Start-Service shotcut
Stop-Service shotcut
Restart-Service shotcut

# Check status
Get-Service shotcut
```

## Advanced Configuration

See the official documentation for advanced configuration options.

## Reverse Proxy Setup

### nginx Configuration

```nginx
upstream shotcut_backend {
    server 127.0.0.1:N/A;
}

server {
    listen 80;
    server_name shotcut.example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name shotcut.example.com;

    ssl_certificate /etc/ssl/certs/shotcut.example.com.crt;
    ssl_certificate_key /etc/ssl/private/shotcut.example.com.key;

    location / {
        proxy_pass http://shotcut_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Apache Configuration

```apache
<VirtualHost *:80>
    ServerName shotcut.example.com
    Redirect permanent / https://shotcut.example.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName shotcut.example.com
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/shotcut.example.com.crt
    SSLCertificateKeyFile /etc/ssl/private/shotcut.example.com.key
    
    ProxyRequests Off
    ProxyPreserveHost On
    
    ProxyPass / http://127.0.0.1:N/A/
    ProxyPassReverse / http://127.0.0.1:N/A/
</VirtualHost>
```

### HAProxy Configuration

```haproxy
frontend shotcut_frontend
    bind *:80
    bind *:443 ssl crt /etc/ssl/certs/shotcut.pem
    redirect scheme https if !{ ssl_fc }
    default_backend shotcut_backend

backend shotcut_backend
    balance roundrobin
    server shotcut1 127.0.0.1:N/A check
```

## Security Configuration

### Basic Security Setup

```bash
# Set appropriate permissions
sudo chown -R shotcut:shotcut /etc/shotcut
sudo chmod 750 /etc/shotcut

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Enable SELinux policies (if applicable)
sudo setsebool -P httpd_can_network_connect on
```

## Database Setup

See official documentation for database configuration requirements.

## Performance Optimization

### System Tuning

```bash
# Basic system tuning
echo 'net.core.somaxconn = 65535' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv4.tcp_max_syn_backlog = 65535' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## Monitoring

### Basic Monitoring

```bash
# Check service status
sudo systemctl status shotcut

# View logs
sudo journalctl -u shotcut -f

# Monitor resource usage
top -p $(pgrep shotcut)
```

## 9. Backup and Restore

### Backup Script

```bash
#!/bin/bash
# Basic backup script
BACKUP_DIR="/backup/shotcut"
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/shotcut-backup-$DATE.tar.gz" /etc/shotcut /var/lib/shotcut

echo "Backup completed: $BACKUP_DIR/shotcut-backup-$DATE.tar.gz"
```

### Restore Procedure

```bash
# Stop service
sudo systemctl stop shotcut

# Restore from backup
tar -xzf /backup/shotcut/shotcut-backup-*.tar.gz -C /

# Start service
sudo systemctl start shotcut
```

## 6. Troubleshooting

### Common Issues

1. **Service won't start**:
```bash
# Check logs
sudo journalctl -u shotcut -n 100
sudo tail -f /var/log/shotcut/shotcut.log

# Check configuration
shotcut --version

# Check permissions
ls -la /etc/shotcut
```

2. **Connection issues**:
```bash
# Check if service is listening
sudo ss -tlnp | grep N/A

# Test connectivity
telnet localhost N/A

# Check firewall
sudo firewall-cmd --list-all
```

3. **Performance issues**:
```bash
# Check resource usage
top -p $(pgrep shotcut)

# Check disk I/O
iotop -p $(pgrep shotcut)

# Check connections
ss -an | grep N/A
```

## Integration Examples

### Docker Compose Example

```yaml
version: '3.8'
services:
  shotcut:
    image: shotcut:latest
    ports:
      - "N/A:N/A"
    volumes:
      - ./config:/etc/shotcut
      - ./data:/var/lib/shotcut
    restart: unless-stopped
```

## Maintenance

### Update Procedures

```bash
# RHEL/CentOS/Rocky/AlmaLinux
sudo dnf update shotcut

# Debian/Ubuntu
sudo apt update && sudo apt upgrade shotcut

# Arch Linux
sudo pacman -Syu shotcut

# Alpine Linux
apk update && apk upgrade shotcut

# openSUSE
sudo zypper update shotcut

# FreeBSD
pkg update && pkg upgrade shotcut

# Always backup before updates
tar -czf /backup/shotcut-pre-update-$(date +%Y%m%d).tar.gz /etc/shotcut

# Restart after updates
sudo systemctl restart shotcut
```

### Regular Maintenance

```bash
# Log rotation
sudo logrotate -f /etc/logrotate.d/shotcut

# Clean old logs
find /var/log/shotcut -name "*.log" -mtime +30 -delete

# Check disk usage
du -sh /var/lib/shotcut
```

## Additional Resources

- Official Documentation: https://docs.shotcut.org/
- GitHub Repository: https://github.com/shotcut/shotcut
- Community Forum: https://forum.shotcut.org/
- Best Practices Guide: https://docs.shotcut.org/best-practices

---

**Note:** This guide is part of the [HowToMgr](https://howtomgr.github.io) collection. Always refer to official documentation for the most up-to-date information.
