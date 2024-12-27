# ProxySQL Installation and Configuration Guide

This guide provides instructions for installing ProxySQL on various Linux distributions and Docker, including details for adding repositories, installing specific versions, and configuring ProxySQL.

## Ubuntu / Debian

### Adding the Repository:
```bash
apt-get install -y --no-install-recommends lsb-release wget apt-transport-https ca-certificates gnupg
wget -O - 'https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/repo_pub_key' | apt-key add -
echo deb https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/$(lsb_release -sc)/ ./ | tee /etc/apt/sources.list.d/proxysql.list
```

For older versions:
- **2.6.x**: `https://repo.proxysql.com/ProxySQL/proxysql-2.6.x/$(lsb_release -sc)/ ./`
- **2.5.x**: `https://repo.proxysql.com/ProxySQL/proxysql-2.5.x/$(lsb_release -sc)/ ./`
- **2.4.x**: `https://repo.proxysql.com/ProxySQL/proxysql-2.4.x/$(lsb_release -sc)/ ./`
- **2.3.x**: `https://repo.proxysql.com/ProxySQL/proxysql-2.3.x/$(lsb_release -sc)/ ./`
- **2.2.x**: `https://repo.proxysql.com/ProxySQL/proxysql-2.2.x/$(lsb_release -sc)/ ./`

Alternatively, add the repository key without `apt-key`:
```bash
wget -nv -O /etc/apt/trusted.gpg.d/proxysql-2.7.x-keyring.gpg 'https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/repo_pub_key.gpg'
```

### Installing ProxySQL:
```bash
apt-get update
apt-get install proxysql
```

For specific versions:
```bash
apt-get install proxysql=[version]
```

---

## RedHat / CentOS

### Adding the Repository:
```bash
cat <<EOF | tee /etc/yum.repos.d/proxysql.repo
[proxysql_repo]
name=ProxySQL repository
baseurl=https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/centos/$releasever
gpgcheck=1
gpgkey=https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/repo_pub_key
EOF
```

For older versions:
- **2.6.x**: `https://repo.proxysql.com/ProxySQL/proxysql-2.6.x/centos/$releasever`
- **2.5.x**: `https://repo.proxysql.com/ProxySQL/proxysql-2.5.x/centos/$releasever`
- **2.4.x**: `https://repo.proxysql.com/ProxySQL/proxysql-2.4.x/centos/$releasever`
- **2.3.x**: `https://repo.proxysql.com/ProxySQL/proxysql-2.3.x/centos/$releasever`
- **2.2.x**: `https://repo.proxysql.com/ProxySQL/proxysql-2.2.x/centos/$releasever`

### Installing ProxySQL:
```bash
yum install proxysql
```

For specific versions:
```bash
yum install proxysql-[version]
```

---

## Amazon Linux

Use CentOS packages based on the version:
- **Amazon Linux 2023**: Use CentOS 8 repository.
- **Amazon Linux 2**: Use CentOS 7 repository.

### Adding the Repository:
```bash
cat <<EOF | tee /etc/yum.repos.d/proxysql.repo
[proxysql_repo]
name=ProxySQL repository
baseurl=https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/centos/8
gpgcheck=1
gpgkey=https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/repo_pub_key
EOF
```

### Installing ProxySQL:
```bash
yum install proxysql
```

For specific versions:
```bash
yum install proxysql-[version]
```

---

## Almalinux

### Adding the Repository:
```bash
cat <<EOF | tee /etc/yum.repos.d/proxysql.repo
[proxysql_repo]
name=ProxySQL repository
baseurl=https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/almalinux/$releasever
gpgcheck=1
gpgkey=https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/repo_pub_key
EOF
```

### Installing ProxySQL:
```bash
yum install proxysql
```

For specific versions:
```bash
yum install proxysql-[version]
```

---

## OpenSUSE Leap

### Adding the Repository:
```bash
cat <<EOF | tee /etc/yum.repos.d/proxysql.repo
[proxysql_repo]
name=ProxySQL repository
baseurl=https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/opensuse/$releasever_major
gpgcheck=1
gpgkey=https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/repo_pub_key
EOF
```

Alternatively:
```bash
zypper addrepo -g -n 'ProxySQL repository' 'https://repo.proxysql.com/ProxySQL/proxysql-2.7.x/opensuse/$releasever_major' proxysql
```

### Installing ProxySQL:
```bash
zypper --gpg-auto-import-keys install proxysql
```

For specific versions:
```bash
zypper --gpg-auto-import-keys install proxysql-[version]
```

---

## Running ProxySQL with Docker

### Prepare a Configuration File
Create a configuration file with the following content (e.g., `/path/to/proxysql.cnf`):
```text
datadir="/var/lib/proxysql"

admin_variables=
{
    admin_credentials="admin:admin;radmin:radmin"
    mysql_ifaces="0.0.0.0:6032"
}

mysql_variables=
{
    threads=4
    max_connections=2048
    default_query_delay=0
    default_query_timeout=36000000
    have_compress=true
    poll_timeout=2000
    interfaces="0.0.0.0:6033"
    default_schema="information_schema"
    stacksize=1048576
    server_version="5.5.30"
    connect_timeout_server=3000
    monitor_username="monitor"
    monitor_password="monitor"
    monitor_history=600000
    monitor_connect_interval=60000
    monitor_ping_interval=10000
    monitor_read_only_interval=1500
    monitor_read_only_timeout=500
    ping_interval_server_msec=120000
    ping_timeout_server=500
    commands_stats=true
    sessions_sort=true
    connect_retries_on_failure=10
}
```

### Run ProxySQL in Docker
Use the following command to start the container:
```bash
docker run -p 16032:6032 -p 16033:6033 -p 16070:6070 -d -v /path/to/proxysql.cnf:/etc/proxysql.cnf proxysql
