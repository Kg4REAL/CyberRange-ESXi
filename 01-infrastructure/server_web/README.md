# 🌐 Web Application Deployment & Insecure Site Setup within Active Directory

This module documents the deployment of the corporate web services and the integration of a deliberately vulnerable web application (**bWAPP**) directly into the Windows Active Directory domain infrastructure.

## 🏗️ Architecture & Lab Specifications
Unlike standard decoupled infrastructures, the web server is joined to the internal Active Directory domain. This design creates a realistic, high-risk enterprise environment where a single web vulnerability can serve as the Initial Access vector to jeopardize domain security.

* **Operating System**: Windows Server 2022 Standard (Domain Member / Active Directory Context)
* **Web Server**: Internet Information Services (IIS)
* **Application Stack**: IIS + FastCGI PHP 8.x + MySQL Server 8.0
* **Target Application**: bWAPP (buggy Web Application)
* **Network Zone**: Internal LAN / Corporate Domain Network

---

## 🛠️ Installation & Configuration Workflow

### 1. Enabling the IIS Web Server Role
The native Windows web server was deployed via **Server Manager** by enabling the following roles and features:
* **Web Server (IIS)**
* **Application Development**: **CGI** (Required to map and execute PHP via FastCGI).

### 2. PHP 8.x Integration for IIS
Since bWAPP is a legacy application written for older PHP versions, deploying it on a modern architecture required configuring the **URL Rewrite** extension and mapping the PHP 8.x FastCGI executable within IIS Manager.

The `php.ini` file was modified to enable database communication:
```ini
extension=mysqli
extension=pdo_mysql
```

![server_web](../../06-reports/screenshots/bwAPP_web_vulnerable.png)
