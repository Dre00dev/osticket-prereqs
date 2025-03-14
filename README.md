# osTicket Installation Guide on Azure Windows VM

## Introduction
Learn to deploy and configure osTicket - an open-source support ticketing system. This guide covers:
- Azure VM creation
- RDP connection
- IIS configuration
- osTicket installation
- Post-installation setup

## Prerequisites
- [Azure free account](https://azure.microsoft.com/free)
- Download this folder:
  - [LINK](https://drive.google.com/drive/u/0/folders/1APMfNyfNzcxZC6EzdaNfdZsUwxWYChf6 )
  - 
## Step 1: Create Azure VM
1. In Azure Portal:
   - **Resource Group**: Create new `ticket-grp`
   - **VM Name**: `osticket-vm`
   - **Region**: (US) East US
   - **Availability**: Zone 1
   - **Image**: Windows 10 Pro 22H2
   - **Size**: Standard_D4s_v3 (4 vCPUs)
   - **Admin Account**: Set username/password
   - **Inbound Ports**: Keep defaults
2. Validate and create VM

## Step 2: Connect via RDP
1. Open Remote Desktop Connection
2. Enter VM's public IP
3. Use admin credentials:
4. Username: [Your Azure VM Username]
5. Password: [Your Azure VM Password]

5. Accept certificate to connect

## Step 3: Configure IIS with CGI
1. Open **Control Panel** > **Programs** > **Turn Windows features on**
2. Enable:
- Internet Information Services
- World Wide Web Services > Application Development Features > **CGI**
3. Click OK to install
![Capture](https://github.com/user-attachments/assets/d41aafde-ef14-4377-8f7b-9eb05f808d90)

## Step 4: Install Required Components
### PHP Manager
1. Run `PHPManagerForIIS-v1.5.0.msi`
2. Follow default installation
![php](https://github.com/user-attachments/assets/08bbc297-eff2-4aa7-a371-08e9517d6ea0)

### Rewrite Module
1. Install `rewrite_amd64`
2. Accept license agreement
![amd rewrite](https://github.com/user-attachments/assets/3aceb4fd-c86e-4a34-8ab2-705f342227b5)

### PHP Setup
1. Create `C:\PHP` folder
2. Extract PHP 7.3.8 files into this folder
![php folder](https://github.com/user-attachments/assets/6fe50484-9b05-4574-be5b-a409d12d8a93)

### MySQL Installation
1. Run `mysql-5.5.62.exe`
2. Choose **Typical** install
3. Complete configuration:
- Set root password
- Install as Windows Service
![msql](https://github.com/user-attachments/assets/64010fc0-b07f-482d-8f1e-fac49e66f4be)
![msql 2](https://github.com/user-attachments/assets/02ef5756-3ab0-4351-8182-b49c27c1d3e5)
![Capturemysql3](https://github.com/user-attachments/assets/410a8c73-b84a-42c4-95ce-40185f59a26d)

### HeidiSQL Setup
1. Install `HeidiSQL_12.3.0.6589_setup.exe`
2. Create database:
- Connect using root credentials
- Right-click > Create new database `osticket_db`
![heidisql2](https://github.com/user-attachments/assets/ef43cad9-986b-4f67-90e6-cb1f32260f07)
![rightclickheidi](https://github.com/user-attachments/assets/60cec4d6-3d76-4dab-b05c-3aed4bb467f1)

## Step 5: osTicket Installation
### File Preparation
1. Extract `osTicket-v1.15.8.zip`
2. Move `upload` folder to `C:\inetpub\wwwroot`
3. Rename folder to `osTicket`

### Configuration
1. Rename file:
- C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php → ost-config.php

2. Set permissions:
- Right-click `ost-config.php` > Properties > Security
- Remove inheritance > Add **Everyone** with Full Control

### IIS Configuration
1. Open IIS Manager as Administrator
2. Register PHP:
- PHP Manager > Register New PHP Version
- Browse to `C:\PHP\php-cgi.exe`
3. Enable extensions:
- php_imap.dll
- php_intl.dll 
- php_opcache.dll
4. Restart IIS
![php manager double click](https://github.com/user-attachments/assets/a9f21c00-a31d-4428-9711-5e8e58751fb4)
![register newphpversion](https://github.com/user-attachments/assets/7842ed36-9e7e-44eb-997f-2af1e7a3f448)
![enable disable php extensions](https://github.com/user-attachments/assets/32d4719f-334f-4b6f-9763-821af8a353bb)

## Step 6: Web Installation
1. Access `http://localhost/osTicket`
2. Complete form:
- **System Settings**
  - Helpdesk Name: [Your Name] Helpdesk
  - Default Email: [your@email.com]
- **Admin User**
  - Name: [Your Name]
  - Email: [admin@email.com]
- **Database Settings**
  - MySQL Database: `osticket_db`
  - MySQL Username: `root`
  - MySQL Password: [Your MySQL Password]
![osticketbasicinstall settings](https://github.com/user-attachments/assets/21ee276c-e612-4d4a-80e2-2df2125bac17)

## Post-Installation
1. Delete setup folder:
  - C:\inetpub\wwwroot\osTicket\setup

2. Secure permissions:
- Set `ost-config.php` to Read/Execute only

Access admin panel: `http://localhost/osTicket/scp`
![osticketinstallsuccess](https://github.com/user-attachments/assets/b993a6c9-c2ae-47a1-8f00-12251a9a6e75)
![congratsosticketinstalldone](https://github.com/user-attachments/assets/c9bc734d-2f4d-4757-b8f3-ca30d58a319e)

Congrats you have set up OSTICKET
