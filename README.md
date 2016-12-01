# How to set up a Hadoop cluster on DigitalOcean
Instructions and tips for setting up a hadoop cluster on digital ocean

This is essentially a boiling down of instructions found here:
https://dwbi.org/etl/bigdata/183-setup-hadoop-cluster

with some additional tips and pitfalls you may encounter


##Create a droplet on DigitalOcean
* Choose Ubuntu 16.04.1 x64
* Choose a 2GB/40GB size(you can choose smaller, but you will likely run into out of heap memory issues
* Select a datacenter. Make note of the region you choose. You will need to select the same region for inter cluster communication.
* Select "Private Networking" in additional options
* Select your ssh key for password-less login
* Enter "NameNode" as the droplet name
* Click "create"

###Set up hosts file
```
vim /etc/hosts
```

Enter the private IP address of the droplet. We need to add the private IP to facilitate droplet to droplet communication within DigitalOcean.

To find your private IP address run the following on the command line
```
root@NameNode: ~# ifconfig
```

Your private IP will appear under the "eth1" section of the output. Copy the private IP and add it to your hosts file.

```
root@NameNode: ~# vim /etc/hosts
```

In the hosts file delete all other entries, and add the following line:

```
192.168.1.1 NameNode
```

where 192.168.1.1 is the private IP of your droplet.

##Tips

###NameNode not starting up or stopping on it's own or failing silently
It could be your NameNode has run out of heap space. You can allocate more memory by adding the following to mapred-site.xml
```
<property>
    <name>mapred.child.java.opts</name>
    <value>Xmx1024m</value>
</property>
```

You may need to allocate additional memory depending on your needs.

###DataNodes not starting up or stopping on their own or failinlg silently
The problem could be that the namenode was formatted after the cluster was set up and the datanodes were not, so the slaves are still referring to the old namenode.

We have to delete and recreate the folder /home/hadoop/dfs/data on the local filesystem for the datanode.

Check your hdfs-site.xml file to see where dfs.data.dir is pointing to
and delete that folder
and then restart the datanode daemon on the machine
