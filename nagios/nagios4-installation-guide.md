# Nagios Core 4 Installation & Web Interface Setup (Ubuntu/WSL)

## 1. Prerequisites

- Ubuntu or WSL terminal
- `sudo` privileges

### Update and install dependencies:

```bash
sudo apt update
sudo apt install -y build-essential libgd-dev openssl libssl-dev unzip apache2 php libapache2-mod-php wget
```

## 2. Create Nagios User/Group

```bash
sudo useradd nagios
sudo groupadd nagcmd
sudo usermod -aG nagcmd nagios
sudo usermod -aG nagcmd www-data
```

## 3. Download Nagios Core

Check the [Nagios official download page](https://www.nagios.org/downloads/nagios-core/) for the latest version.

```bash
cd /tmp
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.1.tar.gz
tar -zxvf nagios-4.5.1.tar.gz
cd nagios-4.5.1
```

## 4. Compile and Install Nagios

```bash
./configure --with-command-group=nagcmd
make all
sudo make install
sudo make install-init
sudo make install-commandmode
sudo make install-config
sudo make install-webconf
```

## 5. Set Nagios Admin Password

```bash
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

## 6. Install Nagios Plugins

```bash
cd /tmp
wget https://nagios-plugins.org/download/nagios-plugins-2.4.6.tar.gz
tar -zxvf nagios-plugins-2.4.6.tar.gz
cd nagios-plugins-2.4.6
./configure --with-nagios-user=nagios --with-nagios-group=nagios --with-command-group=nagcmd
make
sudo make install
```

## 7. Enable Apache CGI Module

```bash
sudo a2enmod cgi
sudo service apache2 restart
```

## 8. Verify and Fix Apache Nagios Config

Ensure `/etc/apache2/conf-available/nagios.conf` exists and has:

```apache
ScriptAlias /nagios/cgi-bin /usr/local/nagios/sbin

<Directory "/usr/local/nagios/sbin">
    Options ExecCGI
    AllowOverride None
    Require all granted
    AuthType Basic
    AuthName "Nagios Access"
    AuthUserFile /usr/local/nagios/etc/htpasswd.users
    require valid-user
</Directory>

Alias /nagios /usr/local/nagios/share

<Directory "/usr/local/nagios/share">
    Options None
    AllowOverride None
    Require all granted
    AuthType Basic
    AuthName "Nagios Access"
    AuthUserFile /usr/local/nagios/etc/htpasswd.users
    require valid-user
</Directory>
```

Enable config and restart Apache:

```bash
sudo a2enconf nagios
sudo service apache2 reload
```

## 9. Fix Permissions (Crucial for WSL)

```bash
sudo chown -R nagios:nagios /usr/local/nagios
sudo chmod -R 775 /usr/local/nagios
```

## 10. Start Nagios

```bash
sudo service nagios start
```
Or, if available:
```bash
sudo systemctl start nagios
```

## 11. Troubleshooting (Common Issues)

### - **Web UI shows “Not running”**
Check Nagios status:
```bash
sudo service nagios status
```
Check permissions:
```bash
sudo chown -R nagios:nagios /usr/local/nagios/var
sudo chmod -R 775 /usr/local/nagios/var
```
Restart services.

### - **Links download HTML instead of rendering**
Enable CGI module (see step 7).

### - **Permission denied errors**
Fix ownership/permissions (see step 9).

### - **Password issues**
Recreate admin password:
```bash
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

### - **Pre-flight config errors**
Run:
```bash
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```
Fix any reported issues.

## 12. Access the Web Interface

Open your browser to:
```
http://localhost/nagios
```
Login with:
- Username: `nagiosadmin`
- Password: (the one you set)

---

**Reference:**  
- [Nagios Core Documentation](https://www.nagios.org/documentation/)
- [Nagios Plugins](https://nagios-plugins.org/)
