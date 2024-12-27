# ProxySQL Installation Guide

## Introduction
In this comprehensive guide, we’ll delve into the intricacies of installing ProxySQL, walking you through the process step by step. From laying down the groundwork and installation procedures to advanced configurations and testing, this guide aims to equip you with the knowledge and confidence to harness the power of ProxySQL effectively. So, buckle up as we embark on a journey to unlock the true potential of your database infrastructure with ProxySQL.

## Pre-Requirements for Installing ProxySQL
Before diving into the installation process of ProxySQL, ensure your system meets the following prerequisites:

- **Supported Operating Systems**: Compatible with Linux distributions like Ubuntu, Debian, CentOS, and RHEL.
- **System Requirements**: Sufficient RAM, CPU, and disk space based on workload and database size.
- **MySQL/MariaDB Installation**: Ensure MySQL/MariaDB is installed and reachable.
- **Network Configuration**: Verify connectivity between ProxySQL and MySQL/MariaDB servers.
- **Access Privileges**: Administrative privileges are required.
- **Package Managers and Dependencies**: Install and update `apt` (Debian/Ubuntu) or `yum` (CentOS/RHEL).
- **DNS Configuration (Optional)**: Ensure DNS resolution is configured if using hostnames for backend servers.

## Decide Installation Method
Choose between installing ProxySQL from source or using a package manager:

### Installing from Source
#### Advantages
- Latest version
- Customization
- Full control

#### Considerations
- Complexity
- Manual dependency handling
- Maintenance responsibility

### Using a Package Manager
#### Advantages
- Ease of installation
- Convenience
- Automatic dependency management

#### Considerations
- Version availability
- Limited customization
- Delayed updates

## Installation from Source
1. **Download the Source Code**:
   ```bash
   git clone https://github.com/sysown/proxysql.git
