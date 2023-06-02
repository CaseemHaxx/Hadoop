
![Logo](https://upload.wikimedia.org/wikipedia/commons/0/0e/Hadoop_logo.svg)

# Hadoop Installation

## Setting up Hadoop v2.9.0 on Ubuntu

This guide provides step-by-step instructions for setting up Hadoop version 2.9.0 on Ubuntu. Hadoop is a powerful open-source framework for distributed storage and processing of large-scale data sets. By following these instructions, you will be able to install and configure Hadoop on your Ubuntu system.


### The setup process includes the following steps:

-   Installing the Java Development Kit (JDK): This step ensures that you have the necessary Java environment to run Hadoop.

-   Installing OpenSSH Server: OpenSSH is required for remote access and communication between Hadoop nodes.

-   Downloading and configuring Hadoop: The Hadoop distribution package will be downloaded and extracted. The necessary environment variables will be set, including the JAVA_HOME and HADOOP_HOME paths.

-   Configuring Hadoop XML files: The core-site.xml and hdfs-site.xml files will be created and configured to specify Hadoop settings such as the default file system and replication factor.

After completing these steps, you will have a fully functional Hadoop installation on your Ubuntu system. This will allow you to perform distributed data processing and storage tasks using Hadoop's powerful ecosystem of tools and libraries.

Please note that this guide assumes you are using Ubuntu as your operating system and Hadoop version 2.9.0. Some details may vary if you are using a different version of Ubuntu or a different version of Hadoop.


# Installation

### Creating Hadoop User and Group

This command adds a new group named "hadoop" to the system.

```bash
sudo addgroup hadoop
```

This command creates a new user named "hadoopusr" and adds it to the "hadoop" and "sudo", granting it administrative privileges group. 
```bash
sudo useradd -m -d /home/hadoopusr -s /bin/bash -G sudo,hadoop hadoopusr
```

This command prompts for changing password of "hadoopusr".
```bash
sudo passwd hadoopusr
```
‼️Logout from current user and sign in with hadoop user.

To switch to the "hadoopusr" user account with superuser privileges, use the following command:
```bash
sudo su - hadoopusr
```

### Update Packages and Install Java Development Kit (JDK)
- To ensure that your system is up to date and has the necessary Java environment, follow these steps:

Update package information:
```bash
sudo apt-get update
```

Check the installed version of Java:
```bash
java -version
```

Install the default JDK:
```bash
sudo apt-get install default-jdk -y
```

### Install OpenSSH Server and Set Up SSH Keys

- To enable remote access and set up SSH keys for secure communication, perform the following steps:

Install the OpenSSH server:
```bash
sudo apt-get install openssh-server -y
```

Generate an RSA key pair (press Enter to use the default file locations):
```bash
ssh-keygen -t rsa -P ""
```

Append the generated public key to the authorized keys file:
```bash
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
```

Verify SSH connectivity by connecting to localhost:
```bash
ssh localhost
```
```bash
exit
```

### Download and Install Hadoop

- To download and install Hadoop version 2.9.0, follow these steps:

Change the directory to the Downloads folder:
```bash
cd /home/hadoopusr/Downloads
```

Download the Hadoop distribution package:
```bash
wget https://archive.apache.org/dist/hadoop/common/hadoop-2.9.0/hadoop-2.9.0.tar.gz
```

- You don't need to follow the above two steps if you have already downloaded hadoop v2.9.0 

Extract the downloaded package:
```bash
sudo tar xvzf hadoop-2.9.0.tar.gz
```

Move the extracted Hadoop directory to the /usr/local/hadoop path:
```bash
mv hadoop-2.9.0 hadoop && sudo mv hadoop /usr/local/
```

By following these steps, you will download the Hadoop distribution package, extract it, and move it to the appropriate installation directory.


### Set Ownership for Hadoop Installation Directory

To ensure that the user "hadoopusr" has the appropriate ownership and permissions for the Hadoop installation directory, execute the following command:
```bash
sudo chown -R hadoopusr /usr/local
```

### Configure Environment Variables for Hadoop

- To configure the necessary environment variables for Hadoop, follow these steps:

Append the following lines to the ~/.bashrc file using the echo command:
```bash
echo 'export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64' >> ~/.bashrc && echo 'export HADOOP_HOME=/usr/local/hadoop' >> ~/.bashrc && echo 'export PATH=$PATH:$HADOOP_HOME/bin' >> ~/.bashrc && echo 'export PATH=$PATH:$HADOOP_HOME/sbin' >> ~/.bashrc && echo 'export HADOOP_MAPRED_HOME=$HADOOP_HOME' >> ~/.bashrc && echo 'export HADOOP_COMMON_HOME=$HADOOP_HOME' >> ~/.bashrc && echo 'export HADOOP_HDFS_HOME=$HADOOP_HOME' >> ~/.bashrc && echo 'export YARN_HOME=$HADOOP_HOME' >> ~/.bashrc && echo 'export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native' >> ~/.bashrc && echo 'export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"' >> ~/.bashrc
```

Load the updated ~/.bashrc file to apply the changes:
```bash
source ~/.bashrc
```

### Check Java Installation
- To verify the installed Java version and location, perform the following steps:

List the contents of the directory to view available Java installations:
```bash
ls -l /usr/lib/jvm
```

### Update Hadoop Environment Configuration
To update the Hadoop environment configuration file (hadoop-env.sh) with the JAVA_HOME environment variable, execute the following command:
```bash
sudo echo 'export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64' | sudo tee -a /usr/local/hadoop/etc/hadoop/hadoop-env.sh
```

### Configure Hadoop XML Configuration Files
- To configure the XML configuration files for Hadoop, follow these steps:

Change the directory to /usr/local/hadoop/etc/hadoop:
```bash
cd /usr/local/hadoop/etc/hadoop
```

Create or update the core-site.xml file with the following content:
```bash
echo '<configuration>
<property>
  <name>fs.default.name</name>
  <value>hdfs://localhost:9000</value>
</property>
</configuration>' > core-site.xml
```

Create or update the hdfs-site.xml file with the following content:
```bash
echo '<configuration>
<property>
  <name>dfs.replication</name>
  <value>1</value>
</property>
<property>
  <name>dfs.namenode.name.dir</name>
  <value>file:/usr/local/hadoop/data/namenode</value>
</property>
<property>
  <name>dfs.datanode.data.dir</name>
  <value>file:/usr/local/hadoop/data/datanode</value>
</property>
</configuration>' > hdfs-site.xml
```

Create or update the yarn-site.xml file with the following content:
```bash
echo '<configuration>
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
  <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
</configuration>' > yarn-site.xml
```

Copy the mapred-site.xml.template file to mapred-site.xml:
```bash
cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template /usr/local/hadoop/etc/hadoop/mapred-site.xml
```

Create or update the mapred-site.xml file with the following content:
```bash
echo '<configuration>
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
</property>
</configuration>' > mapred-site.xml
```

### Create Hadoop Data Directories

- To create the necessary directories for Hadoop data storage, follow these steps:

Create the /usr/local/hadoop_space directory:
```bash
sudo mkdir -p /usr/local/hadoop_space
```

Create the /usr/local/hadoop_space/hdfs/namenode directory:
```bash
sudo mkdir -p /usr/local/hadoop_space/hdfs/namenode
```

Create the /usr/local/hadoop_space/hdfs/datanode directory:
```bash
sudo mkdir -p /usr/local/hadoop_space/hdfs/datanode
```

### Format the Hadoop Namenode

To format the Hadoop namenode, run the following command:
```bash
hdfs namenode -format
```

### Start Hadoop Services

- To start the Hadoop services and check their status, follow these steps:

Start the Hadoop Distributed File System (HDFS) by running the following command:
```bash
start-dfs.sh
```

Start the YARN resource manager and nodemanager by executing the following command:
```bash
start-yarn.sh
```

Check the status of the running Java processes using the jps command:
```bash
jps
```

### Accessing Hadoop Web Interface

To access the Hadoop web interface and monitor the Hadoop cluster, enter the following URL:

[http://localhost:50070](http://localhost:50070)

### Stopping Hadoop Services

Stop the Hadoop Distributed File System (HDFS) services by running the following command:
```bash
stop-dfs.sh
```

Stop the YARN services by executing the following command:
```bash
stop-yarn.sh
```


## Compiled by:

- [Muhammad Qaseem](https://github.com/CaseemHaxx)

<h1 align="center">
  Let's Connect and have a Chat!💬
</h1>
<p align="center">
<a href="https://pk.linkedin.com/in/Muhammad-Qaseem/">
  <img height="50" src="https://user-images.githubusercontent.com/46517096/166973395-19676cd8-f8ec-4abf-83ff-da8243505b82.png"/>
<a href="https://medium.com/@Rorsch4ch">
  <img height="50" src="https://user-images.githubusercontent.com/46517096/166973962-d05d145a-b6a0-4643-bd3d-5ac845679367.png"/>
<a href="https://tryhackme.com/p/Rorsch4ch">
  <img height="50" src="https://assets.tryhackme.com/img/favicon.png"/>
</a>
<a href="https://twitter.com/Rorschach_0x01">
  <img height="50" src="https://user-images.githubusercontent.com/46517096/166974271-91dfa250-d70b-4cb9-8707-f1bda1b708c3.png"/>
</a>
<a href="https://www.instagram.com/caseem._.haxx">
  <img height="50" src="https://user-images.githubusercontent.com/46517096/166974368-9798f39f-1f46-499c-b14e-81f0a3f83a06.png"/>
</a>
</p>
