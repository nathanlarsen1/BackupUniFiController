<h1>Backup UniFi Controller Configuration Files to Windows Share</h1>


<h2>Description</h2>
This project consists of an Bash script that backs up the configuration files for Unifi Controller to a Windows Share.<br/>

<h2>Language and Applications</h2>

- <b>Bash</b>
- <b>Unifi Controller</b>

<h2>Environments Used </h2>

- <b>Ubuntu Server 22.04.4 LTS</b>
- <b>Windows Server 2019 Standard</b>

<h2>Setup</h2>

- Create shared folder and user on Windows Server</br>
  - Setup shared folder named UC_Backup.
  - Create user account with name of your choosing.
  - Grant user write priveleges to shared folder.

- Setup backup script to run at desired time on Ubuntu Server</br>
  - Create /opt/backup directory:</br>
    $ sudo mkdir /opt/backup

  - Create /opt/backup/backup_uc.sh and copy contents of backup_uc.sh.txt into /opt/backup/backup_uc.sh:</br>
    $ sudo nano /opt/backup/backup_uc.sh

  - Grant the execute permissions to /opt/backup/backup_uc.sh:</br>
    $ sudo chmod +x /opt/backup/backup_uc.sh

  - Setup Cron Job to run the backup script at selected time:</br>
    $ sudo crontab -e

  - Add the following lines to crontab:

    <span>#</span> Run backup_uc.sh every day at 1:00am.</br>
    0 1 * * * /opt/backup/backup_uc.sh > /dev/null 2>&1

- Install and setup CIFS Utilities Package on Ubuntu Server</br>
  - $ sudo apt update
  - $ sudo apt install cifs-utils
    
  - Create /etc/cifs-credentials and copy contents of cifs-credentials.txt into /etc/cifs-credentials.</br>
    Change username, password and domain to match the credentials of the user created earlier on the Windows Server:</br>
    $ sudo nano /etc/cifs-credentials
    
  - Set the proper permissions to secure the /etc/cifs-credentials file:</br>
    $ sudo chmod 600 /etc/cifs-credentials

- Setup automatic mounting of Windows share on boot on Ubuntu Server</br>
  - Create /mnt/ucbackup directory:</br>
    $ sudo mkdir /mnt/ucbackup
    
  - Set proper permissions for mountpoint:</br>
    $ sudo chmod 600 /mnt/ucbackup
    
  - Setup fstab to mount the Windows share on boot:</br>
    $ sudo nano /etc/fstab

  - Add the following lines to fstab:</br>
  
    <span>#</span> mount UC_Backup share to /mnt/ucbackup on boot<br/>
    //\<IP address of Windows Server>/\<Windows Share Name> /mnt/ucbackup cifs iocharset=utf8,credentials=/etc/cifs-credentials,uid=1000 0 0
    
  - Mount all file systems in /etc/fstab:</br>
    $ sudo mount -a
<br />
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
