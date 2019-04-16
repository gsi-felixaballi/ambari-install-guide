# Ambari Install Guide Step-by-step

> ## Step-by-step guide for Ambari Agent/Server deployment

## Docker Registry: Download Debian image

```bash

docker pull debian:latest

docker run -it --name ambari-node -h ambari-node -p 8000:8000 -v /home/i-am/ambari/server-logs/:/var/log/ambari-server -v /home/i-am/ambari/agent-logs:/var/log/ambari-agent debian:ambari

```

## *Warning*
>
> Default port is *'8080'*, preferred *'8000'*
>

### Published *logs* directories

> *Agent (logs)*: **/var/log/ambari-agent**
>
> *Server (logs)*: **/var/log/ambari-server**

---

## Download Ambari repositories

```bash

wget -O /etc/apt/sources.list.d/ambari.list http://public-repo-1.hortonworks.com/ambari/debian9/2.x/updates/2.7.3.0/ambari.list

sudo apt-key adv --recv-keys --keyserver keyserver.debian.com B9733A7A07513CAD

```

---

## Install Ambari Agents manually on Debian 9

### Ambari Agent default settings (**/etc/ambari-agent/conf/ambari-agent.ini**)

```bash
sudo apt update

sudo apt install ambari-agent

nano /etc/ambari-agent/conf/ambari-agent.ini

# Agent Settings
# [server]
# hostname=<your.ambari.server.hostname>
# url_port=8440
# secured_url_port=8441

```

---

## Install Ambari Server manually on Debian 9

### Ambari Server default settings (**/etc/ambari-server/conf/ambari.properties**)

```bash

sudo apt update

apt-cache showpkg ambari-server
                     apt-cache showpkg ambari-agent
                     apt-cache showpkg ambari-metrics-assembly

sudo apt install ambari-server

```

---

## Ambari Server: Setup options

```bash
# Install JDBC library (default: PostgreSQL Embedded Mode)
sudo apt install -y libpostgresql-jdbc-java
```

![PostgreSQL Library](./images/postgresql-library.png)

```bash

ambari-server setup --jdbc-db=postgres --jdbc-driver=/usr/share/java/postgresql-9.4.1212.jar

ambari-server setup

```

![Setup Steps](./images/ambari-server-setup-options-1.png)

![Setup Steps](./images/ambari-server-setup-options-2.png)

## *Ambari Server: default settings*

```bash

# ...
ambari-server.user=root
jdk.name=jdk-8u112-linux-x64.tar.gz
jdk1.8.desc=Oracle JDK 1.8 + Java Cryptography Extension (JCE) Policy Files 8
java.home=/usr/jdk64/jdk1.8.0_112 (downloaded from repositories)
# ...

# Property added
client.api.port=8000 (default: 8080)

# Database section
server.jdbc.database=postgres
server.jdbc.database_name=ambari
server.jdbc.postgres.schema=ambari
server.jdbc.user.name=ambari
server.jdbc.user.passwd=/etc/ambari-server/conf/password.dat

# Server webdir
webapp.dir=/usr/lib/ambari-server/web

```

---

## **(Optional)** Ambari Server: Solve ports conflicts

```bash

# Checks active processes in use by Java
ps -aux | grep java

kill <process-id> #PID

ambari-server start

```

---

## Ambari Server: Start instance

```bash
# Start server instance
ambari-server start

```

---

## Ambari Server: login page

![Ambari Server Login Page](./images/ambari-server-login-page.png)

## *Default Login*

```text
Username: admin
Password: admin
```

---

## Admin Section

![Ambari Server Admin Section](./images/ambari-server-admin-section.png)

---

## Ambari Documentation

- [Ambari: Download repositories](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/administering-ambari/content/amb_download_the_ambari_repository_on_debian_7.html)

- [Ambari: Install agents manually](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/administering-ambari/content/amb_install_the_ambari_agents_manually_on_debian_7.html)
- [Ambari: Install server manually](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/bk_ambari-installation/content/install-ambari-server-debian9.html)

- [Ambari: Setup server](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/bk_ambari-installation/content/set_up_the_ambari_server.html)