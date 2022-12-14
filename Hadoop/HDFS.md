
# HDFS - Hadoop Distributed File System

## Name Node
- It acts like a master node & manages Data Node
- It maintains the metadata of all the files in the cluster. Example: Location of data blocks, file size, location of replicated data

## Data Node
- It is a slave node
- The actual data is stored on Data Nodes
- Data Nodes will also help in computing
- Data Nodes sends heartbeat to Name Node periodically to report the overall health of the system

## Block
- Smallest unit of physical memory where data is stored
- Default size of a block in Hadoop 2.x is 128MB

## Replication Management
- ***Replica*** -> Multiple copies of same block
- ***Replication factor*** -> It indicates total no. of copies of the data block. In Hadoop default replication factor is 3 (1 primary block + 2 replicated blocks). Each replica is stored in different DataNode.

## Rack Awareness
- ***Rack*** -> The rack is a physical collection of nodes in Hadoop cluster (maybe 30 to 40).
- ***Rack awareness policies***
	1. There should not be more than 1 replica on the same Datanode.  
	2. More than 2 replica’s of a single block is not allowed on the same Rack.  
	3. The number of racks used inside a Hadoop cluster must be smaller than the number of replicas.
<img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/20200702122324/HDFS-Rack-Awareness-Example.png" alt="hadoop" width="400" height="400"/>

## HDFS Write Request
- 3 steps of HDFS write request:  
	1. HDFS Client contacts Name Node to get the IP addresses of Data Nodes in which data will be written.  
	2. It inserts & replicates data in the given Data Nodes.  
	3. It sends acknowledgement to Name Node regarding the success/failure of the write operation. Name Node updates Meta data accordingly.

## High Availability or Fault Tolerence

- It is a method to handle the failure/breakdown of Name Node
- Highly Available -> Very very less downtime
- Name node maintains below information
	1. ***Edit Logs*** - It contains the recent modifications made to the file system. It stores only temporary data, it gets empty when its data is copied into FS-Image.
	2. ***FS-Image*** - It contains complete state of the file system since the start of Name Node. After a particular time interval Edit Logs data is appended in FS-Image.

- ***2 ways to achieve High Availability in Hadoop***:
	1. ***Secondary Name Node***: 
		- It does incremental checkpointing(Time interval)
		- Keeps latest FS-Image at checkpoint as a backup file
		- In case of breakdown the Name Node will fetch back the FS-Image data from the Secondary Name Node.
		- Not suitable for critical systems as downtime > 10 mins

	2. ***Standby Name Node***:
		- Duplicate of Name Node
		- Works in Active-Passive mode
		- If Name Node breakdowns the Standby Node will go in Active mode & Name Node will go in Passive mode
		- It is in complete sync with Name Node hence downtime is very less


