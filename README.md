# EAM to MAS (Manage)

> The following is an instructor-led lab.

## 1a. Prepare/start OCP

Log in and download your pull-secret from: [https://console.redhat.com/openshift/downloads#tool-pull-secret](https://console.redhat.com/openshift/downloads#tool-pull-secret)

⏰ 45 minutes hour-glass wait time.

### Open PowrShell and SSH to a bastion node

1.1 Remote connect using Secure Shell.
```shell
ssh admin@192.168.252.2
(password: vCenter password)
```
1.2 Change to root user.
```shell
sudo su -
```
1.3 Start OCP
```shell
/data/daffy/ocp/build.sh vmware-ipi
```
1.4 When prompted, paste your pull-secret and hit enter to continue.

**Leave this terminal running. You will come back to it later when OCP is installed.**

---

## 2. Review Maximo 7.x and prepare customization archive

2.1 Connect to the **BeforeCustomization** server using the available RDP icon from the desktop.

**Interactive**

BeforeCustomization
http://max76vmw.maximo.demo:9080/maximo

AfterCustomization
http://max76vmw.demo.local:9080/maximo

`maxadmin / Passw0rd01`

---

## 3. Prepare Database Migration

⏰ 20 minutes hour-glass wait time.

We have taken a backup of the Maximo 7.x instance using the standard DB2 backup commands. Your task is to create a new database and restore the data.

3.1 Connect to the database server using the available RDP icon `win_db2_server` from the desktop.

3.2 Open DB2 Command Window Administrator and run this script.
```bat
db2_restore.bat
```

**Wait.**

3.3 Using DBeaver installed on your server, test newly created `MAXDB80` database.

|Key  |Value  |
|:----|---|
|Hostname  |localhost  |
|Database  |maxdb80  |
|Username  |db2admin  |
|Password  |Passw0rd01  |
|Port  |50005  |

3.4 Run SQL query to identify Maximo database's version level (at 7.x).
```sql
select varvalue from maxvars where varname = 'MAXUPG';
```

---

## 1b. Complete OCP

1.5 When OCP is installed, you will see a final output on your terminal containing credentials and URLs. Copy and save the content into your password file.

1.6 Log in to OCP using the Console URL and the `ocpadmin` credentials by clicking on **Daffy htpasswd Provider**.

1.7 Ensure green check marks.

1.8 Log in to vCenter to review control-plane/worker nodes.

---

## 4. EAM to MAS Upgrade

⏰ 1 hour 30 minutes.

4.1 Open a new PowerShell window and change directory. We are providing a short term MAS evaluation license for this workshop, which is placed on this directory.
```PowerShell
cd C:\mas8
```

4.2 Run this command to start MASCLI container.
```PowerShell
docker run -it --pull always -v ${PWD}:/mas8 --name ibmmas quay.io/ibmmas/cli:7.14.1
```

4.3 Log in to OCP.

**Interactive**

- Pipeline, customization archive link.
```
https://democore-cos-cos-standard.s3.us-east.cloud-object-storage.appdomain.cloud/custasset_bin.zip
```
|Key  |Value  |
|:----|---|
|MAS_JDBC_USER  |db2admin  |
|MAS_JDBC_PASSWORD  |Passw0rd01  |
|MAS_JDBC_URL  |jdbc:db2://192.168.252.105:50005/maxdb80  |
|Schema  |MAXIMO  |
|Table space  |MAXDATA  |
|Index space  |MAXINDEX  |
