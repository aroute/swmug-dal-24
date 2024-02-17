# swmug-dal-24



## 1. Connect to your Remote Work Environment

Double-click the RDP icon to connect to the remote work environment. Username: `Administrator`, Password: `Passw0rd01`.

**All needed passwords have been provided to you in a password file available on the desktop.**

---

## 2. Prepare for OCP

Log in and download your pull-secret from: [https://console.redhat.com](https://console.redhat.com)

## 3. Start OCP

⏰ 45 minutes hour-glass wait time.

### Open PowrShell and SSH to a bastion node

3.1 Remote connect using Secure Shell.
```shell
ssh admin@192.168.252.2
(password: vCenter password)
```
3.2 Change to root user.
```shell
sudo su -
```
3.3 Start OCP
```shell
/data/daffy/ocp/build.sh vmware-ipi
```
3.4 When prompted, paste your pull-secret and hit enter to continue.

---

## 4. Review Maximo 7.x and prepare customization archive

4.1 Connect to the **BeforeCustomization** server using the available RDP icon from the desktop.

**Interactive**

---

## 5. Prepare Database Migration

⏰ 20 minutes hour-glass wait time.

We have taken a backup of the Maximo 7.x instance using the standard DB2 backup commands. Your task is to create a new database and restore the data.

5.1 Connect to the database server using the available RDP icon from the desktop.

5.2 Open PowerShell to download the restore database script.
```powershell
Invoke-WebRequest -Uri https://gist.githubusercontent.com/aroute/9b41d9cb6e6cd3af341deedcc006bf2a/raw/6039688f10361a614ae3ec40d32bf7d1eb6f16fb/db2_restore.bat -OutFile C:\IBM\SQLLIB\BIN\db2_restore.bat
```
5.3 Open DB2 Command Window Administrator and run this script.
```bat
db2_restore.bat
```

**Wait.**

5.4 Using DBeaver installed on your server, test newly created `MAXDB80` database.
```
hostname: localhost
DB: maxdb80
username: db2admin
password: Passw0rd01
port: 50005
```
5.5 Run SQL query to identify Maximo database's version level.
```sql
select varvalue from maxvars where varname = 'MAXUPG';
```
