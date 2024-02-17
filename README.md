# swmug-dal-24

> The following is an instructor-led lab.

## 1. Connect to your Remote Work Environment

Double-click the RDP icon to connect to your remote work environment. Username: `Administrator`, Password: `Passw0rd01`.

**All needed passwords have been provided to you in a password file available on the desktop.**

---

## 2a. Prepare/start OCP

Log in and download your pull-secret from: [https://console.redhat.com/openshift/downloads#tool-pull-secret](https://console.redhat.com/openshift/downloads#tool-pull-secret)

⏰ 45 minutes hour-glass wait time.

### Open PowrShell and SSH to a bastion node

2.1 Remote connect using Secure Shell.
```shell
ssh admin@192.168.252.2
(password: vCenter password)
```
2.2 Change to root user.
```shell
sudo su -
```
2.3 Start OCP
```shell
/data/daffy/ocp/build.sh vmware-ipi
```
2.4 When prompted, paste your pull-secret and hit enter to continue.

**Leave this terminal running. You will come back to it later when OCP is installed.**

---

## 3. Review Maximo 7.x and prepare customization archive

3.1 Connect to the **BeforeCustomization** server using the available RDP icon from the desktop.

**Interactive**

BeforeCustomization
http://max76vmw.maximo.demo:9080/maximo

AfterCustomization
http://max76vmw.demo.local:9080/maximo

`maxadmin / Passw0rd01`

---

## 4. Prepare Database Migration

⏰ 20 minutes hour-glass wait time.

We have taken a backup of the Maximo 7.x instance using the standard DB2 backup commands. Your task is to create a new database and restore the data.

4.1 Connect to the database server using the available RDP icon `win_db2_server` from the desktop.

4.2 Open DB2 Command Window Administrator and run this script.
```bat
db2_restore.bat
```

**Wait.**

4.3 Using DBeaver installed on your server, test newly created `MAXDB80` database.
```
hostname: localhost
DB: maxdb80
username: db2admin
password: Passw0rd01
port: 50005
```
4.4 Run SQL query to identify Maximo database's version level (at 7.x).
```sql
select varvalue from maxvars where varname = 'MAXUPG';
```

---

## 2b. Complete OCP

2.5 When OCP is installed, you will see a final output on your terminal containing credentials and URLs. Copy and save the content into your password file.

2.6 Log in to OCP using the Console URL and the `ocpadmin` credentials by clicking on **Daffy htpasswd Provider**.

2.7 Ensure green check marks.

2.8 Log in to vCenter to review control-plane/worker nodes.

---

## 5. EAM to MAS Upgrade

⏰ 1 hour 30 minutes.

5.1 Open PowerShell and change directory.
```shell
cd mas8
```

5.2 Run this command to start MASCLI.
```shell
docker run -it --pull always -v ${PWD}:/mas8 --name ibmmas quay.io/ibmmas/cli:7.14.1
```

**Interactive**

- Pipeline, customization archive link.
```
https://democore-cos-cos-standard.s3.us-east.cloud-object-storage.appdomain.cloud/custasset_bin.zip
```
```
MAS_JDBC_USER=db2admin
MAS_JDBC_PASSWORD=Passw0rd01
MAS_JDBC_URL=jdbc:db2://192.168.252.110:50005/maxdb80
Schema=MAXIMO
Table space=MAXDATA
Index space=MAXINDEX
```
