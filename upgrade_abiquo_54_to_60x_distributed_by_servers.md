# Upgrade Abiquo 5.4 to 6.x distributed by servers

## Introduction to distributed upgrade by servers

This section describes how to upgrade from Abiquo 5.4 to any **Abiquo 6.0.x or Abiquo 6.1.x version.** Please contact Abiquo Support for further information.

**This is a non-official upgrade document from which you can easily cut and paste the commands for each to prepare your own upgrade document.**

* **The official upgrade document, that you must read and refer to for your upgrade is still** **[Upgrade Abiquo](https://abiquo.atlassian.net/wiki/display/doc/Upgrade+Abiquo)**

**Abiquo YUM repositories are no longer open, please contact Abiquo Support to obtain your credentials**

**This upgrade process starts from Abiquo 5.4.0 or above.** To upgrade from earlier versions, please see [Upgrade Abiquo from 5.2 to 5.4.x](https://abiquo.atlassian.net/wiki/display/doc/Upgrade+Abiquo+from+5.2+to+5.4.x) or the appropriate page for your version.

* The upgrade will overwrite **lang_en_US.json**. If you have customized labels or translations, back them up before the upgrade and restore them afterwards accordingly.
* The upgrade will overwrite the SAML login configuration for multiple IdPs. If you use SAML with multiple IdPs, back up the configuration files before the upgrade and add the multiple IdPs afterwards accordingly. See [Restore SAML security beans after upgrade](https://abiquo.atlassian.net/wiki/display/doc/Restore+SAML+security+beans+after+upgrade)


## Prevent cloud users from performing VM operations

1. In the UI in the Infrastructure view, select each physical machine and click Disable
2. Using the API, set the state of each physical machine to DISABLED
  
## API servers

On each API Server do these steps. If you have a separate DB server, do the DB upgrade steps on that server.

```bash
# Log in as root
sudo su -

# Check that the Abiquo RabbitMQ queues are empty (API or Datanode).
# The number of messages in all queues must be 0.
# rabbitmqctl list_queues messages name

# Back up the UI
tar cvfz /var/www/html/ui.tgz-$(date +%Y%m%d_%H%M%S) /var/www/html/ui

# Back up the /opt/abiquo folder
tar cvfz /opt/abiquo.tgz-$(date +%Y%m%d_%H%M%S) /opt/abiquo
 
# Back up Redis
redis-cli save
cp -a /var/lib/redis /var/lib/redis-$(date +%Y%m%d_%H%M%S)

# On DB Server, back up the Abiquo MySQL DBs
mysqldump --routines --triggers kinton            > kinton.sql-$(date +%Y%m%d_%H%M%S)
mysqldump --routines --triggers kinton_accounting > kinton_accounting.sql-$(date +%Y%m%d_%H%M%S)

# IF you are using SAML with multiple IdPs, back up security beans files
cp
/opt/abiquo/tomcat/webapps/api/WEB-INF/classes/springresources/security/saml/security-saml-generated-beans.xml /opt/abiquo/tomcat/webapps/api/WEB-INF/classes/springresources/security/saml/security-saml-generated-beans.xml-$(date
 +%Y%m%d_%H%M%S)

# Stop  services
systemctl stop httpd
systemctl stop abiquo-tomcat
 

# Install latest yum release package for your target version using link
yum remove 'abiquo-release-*'
# Find the correct link at https://abiquo.atlassian.net/wiki/spaces/doc/pages/311363475/Abiquo-release-ee+packages+list
# For 6.1
yum localinstall https://user:passwd@mirror.abiquo.com/el7/6.1/os/x86_64/abiquo-release-ee-6.1.0-20230112_211400.el7.noarch.rpm

# Prepare yum repository for your target version
yum-config-manager --enable abiquo-base
yum-config-manager --enable abiquo-updates

 
# Perform update
yum clean all && yum makecache
yum update 'abiquo-*'


# Upgrade the Abiquo databases
abiquo-db [-h DB hostname] [-P DB port] [-u user] [-p password] update


# IF the database upgrade fails because of checksums
abiquo-db [-h DB hostname] [-P DB port] [-u user] [-p password] clearCheckSums
abiquo-db [-h DB hostname] [-P DB port] [-u user] [-p password] update

# In Abiquo 6.1.0, optionally, deactivate on-premise billing
# Log in to the database server as an administrator and run this command in MySQL
update kinton.system_properties set value = 0 where name = "client.billing.onpremise.enabled";

# Post-upgrade step for API
chown -R tomcat /opt/abiquo
```

### Additional API and UI upgrade steps

* Add translations for new [UI labels](https://abiquo.atlassian.net/wiki/display/doc/UI+labels) to lang_xx_XX_custom.json. See [Abiquo UI client language configuration](https://abiquo.atlassian.net/wiki/display/doc/Abiquo+UI+client+language+configuration)
* Optionally, add new configuration to client-config-custom.json. See [Configure Abiquo UI](https://wiki.abiquo.com/display/doc/Configure+Abiquo+UI)
* In your UI configuration, check that the API endpoint is pointing to your Abiquo API server:
```json
{  
    "config.endpoint": "http://<API_IP_ADDRESS>/api" 
}`
```
* Add new API properties to the `abiquo.properties` file found in the `/opt/abiquo/config/` folder. See [Changes to Abiquo Configuration Properties](https://wiki.abiquo.com/display/doc/Changes+to+Abiquo+Configuration+Properties)
 

## Remote services servers

On each remote services server do these steps.

```bash
# Log in as root
sudo su -
 
# On V2V servers, check for active conversions
# ps aux | grep v2v
# ps aux | grep mechadora
 
# Back up the /opt/abiquo folder
tar cvfz /opt/abiquo.tgz-$(date +%Y%m%d_%H%M%S) /opt/abiquo
 
# Back up Redis
redis-cli save
cp -a /var/lib/redis /var/lib/redis-$(date +%Y%m%d_%H%M%S)

# Stop services
systemctl stop abiquo-tomcat

# Check if there is an NFS shared folder for the NARS results (related to system property abiquo.nars.async.fileresults.path):
mount | grep -i "nfs\|results" | grep -v sunrpc
# 10.60.37.87:/abq-pro-nfs on /opt/abiquo/results-nars type nfs4 (rw,nosuid,nodev,noexec,relatime,sync,vers=4.1,rsize=1048576,wsize=1048576,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.60.37.77,local_lock=none,addr=10.60.37.87,user)

#Unmount the previous shared folder if you have it mounted
umount /opt/abiquo/results-nars

# Install latest yum release package for your target version using link
yum remove 'abiquo-release-*'
# Find the correct link at https://abiquo.atlassian.net/wiki/spaces/doc/pages/311363475/Abiquo-release-ee+packages+list
# For 6.1
yum localinstall https://user:passwd@mirror.abiquo.com/el7/6.1/os/x86_64/abiquo-release-ee-6.1.0-20230112_211400.el7.noarch.rpm;

# Prepare yum repository for your target version 
yum-config-manager --enable abiquo-base
yum-config-manager --enable abiquo-updates
  
# Perform update
yum clean all && yum makecache
yum update 'abiquo-*'

# Remount the shared folder for NARS (and any other folders you have unmounted)
mount -a

# Post upgrade steps for RS
chown -R tomcat /opt/abiquo
chown -R tomcat /opt/vm_repository
chmod -R a+r /opt/vm_repository

cd /usr/share/doc/abiquo-redis/redis/
bash ./4.2.3/00-old-vsm-definitions.sh
```

### Additional RS upgrade steps

Add new RS properties to the abiquo.properties file found in the /opt/abiquo/config/ folder. See [Changes to Abiquo Configuration Properties](https://wiki.abiquo.com/display/doc/Changes+to+Abiquo+Configuration+Properties)


## Monitoring servers

On each Watchtower monitoring server do these steps.

```bash
# Log in as root and stop services
sudo su -
systemctl stop abiquo-emmett
systemctl stop abiquo-delorean
systemctl stop kairosdb
systemctl stop cassandra
 
# Really kill Cassandra by getting its process number (e.g. XXXXX) and killing it
ps auwx | grep cassandra
kill -9 XXXXX

# Install latest yum release package for your target version using link
yum remove 'abiquo-release-*'
# Find the correct link at https://abiquo.atlassian.net/wiki/spaces/doc/pages/311363475/Abiquo-release-ee+packages+list
# For 6.1
yum localinstall https://user:passwd@mirror.abiquo.com/el7/6.1/os/x86_64/abiquo-release-ee-6.1.0-20230112_211400.el7.noarch.rpm

# Prepare yum repository for your target version 
yum-config-manager --enable abiquo-base
yum-config-manager --enable abiquo-updates

# Perform update
yum clean all && yum makecache
yum update 'abiquo-*'
 
# Update watchtower database
watchtower-db [-h DB hostname] [-P DB port] [-u user] [-p password] update

```

## Start Abiquo server and services

Start all servers in the correct order.

```bash
# On API Server
service httpd restart
service abiquo-tomcat start
 

# On Remote Services including V2V
service abiquo-tomcat start
 

# On the Monitoring Server, start the Cassandra service
sudo service cassandra start
 

# WAIT about 5 minutes until Cassandra is up and running
# Start the KairosDB service
sudo service kairosdb start
 

# Start the other services in this order
sudo service abiquo-emmett start
sudo service abiquo-delorean start

# In the Abiquo UI, reenable the physical machines

```

## Reporting server

We recommend that you update your reports server to JasperReports v7.8.0.

  
