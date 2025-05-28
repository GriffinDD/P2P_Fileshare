# P2P_Fileshare
A rudimentary P2P system where seeders can share/recieve files from other peers.


# How to Run
Explained below is how to run each component in the P2P Fileshare Software System.  
You may need to enable permission to run powershell scripts, as by default they tend to be blocked from direct execution.
Furthermore, before execution, make sure that you have an output and hashes folder created.  
All peers, regardless of type, must be run in the same location as the 2 helper libraries and the security files.

## Tracker
Acts as a central peer discovery mechanism for the P2P protocol. Stores and distributes the current peers in the swarm. Always running.  
Necessary for when peers first enter a swarm.
```cmd
python .\Tracker.py
```

## Peer
This is the file that creates each peer of each type within the network.  
There are two types of Peers, Seeders and Downloaders.  
Seeders have some or all pieces for each file in the swarm.  
Downloaders have no parts of the file upon joining the swarm and their goal is to get all the pieces of the file.  

**Running a peer**  
```cmd
python .\Peer -i Tracker ip [-S] [-p port range] [-d Tracker port] [-f file] [-m [M ...]] [-r1 R1] [-r2 R2]
```
Arguments:
-p port range: An integer signifying the first port number the peer uses. Using all ports from port range to port range + 4. Default of 9000.  
-i Tracker ip: A required string indicating the IP address of the Tracker. Usually "localhost".  
-d Tracker port: The port of the Tracker. Default of 9999.  
-S: Boolean flag that if used indicated that this peer is a Seeder. Default False.  
-f filename: Name of the file that this peer is interested in. Needed for Downloader. Default of "0short_story.txt".  
-m pieces. List of missing pieces for this Seeder. Space separated group of ints.  
-r1 range: Optional lower range of pieces for this Seeder.  
-r2 range: Optional upper range of pieces for this Seeder.  
Note that the range option will be used even if only one bound is set, with the other defaulting to 0.0 or 1.0 for lower and upper respectively.  
Also if both missing pieces and ranges are provided, the missing pieces will be used.


# Command Line Usage 
We have developed a simple command line interface so users can check the progress of their download, be told when it is done, and exit if needed.  
While the peers are running you can type `prog` to see the current progress of the download.  
Or if you are done, you can exit by typing `exit`.  
When the Downloader is done `"Successfully Downloaded {output_file} to output/{output_file}"`.  
Note: If a critical error with the tracker is encountered (which should really only occur when intentionally turning off the tracker mid-test),
you should print prog or exit to cycle the client to check the exit state, otherwise it will hang waiting for your input.
