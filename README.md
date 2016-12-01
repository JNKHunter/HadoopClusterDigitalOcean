# HadoopClusterDigitalOcean
Instructions and tips for setting up a hadoop cluster on digital ocean

##Create a droplet on DigitalOcean
* Choose Ubuntu 16.04.1 x64
* Choose a 2GB/40GB size(you can choose smaller, but you will likely run into out of heap memory issues
* Select a datacenter. Make note of the region you choose. You will need to select the same region for inter cluster communication.
* Select "Private Networking" in additional options
* Enter "NameNode" as the droplet name
* Click "create"


##Tips

###DataNotes not starting up or stopping on their own
The problem could be that the namenode was formatted after the cluster was set up and the datanodes were not, so the slaves are still referring to the old namenode.

We have to delete and recreate the folder /home/hadoop/dfs/data on the local filesystem for the datanode.

Check your hdfs-site.xml file to see where dfs.data.dir is pointing to
and delete that folder
and then restart the datanode daemon on the machine
