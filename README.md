# Security Analyst - Secure the Payload
## Table of Content
___

- TODO

## Important Notes
___



## Log Sources
___

Log Sources - TO DO 

Inscope - TO DO 

Out of Scope - TO DO
### Log Source Categories
- Antivirus
	- Malware Detected, Update

- Firewall
	- Accept, Deny, Session Closed

- IDS/IPS
	- Monitor, Block, Sessions Closed

- Operating System
	- Registery Modification, Audit Log Cleared, Service Installed, wtc.

- SSO/AD
	- Authenciation Success/Failure, User Createed/Deleted, etc.

- Proxy
	- URL Filtering, etc.
	
### Antivirus
Malware Detected: A maclious file or process was detected on the system

Things you may be able to find in a AV log:

```
Action, Source IP, Destination IP, USer, File Has, File, Name, File Path, Reason, Severity, 
Signatute, Signature Idm Source Hostm Destination Host, Category
```

### Firewall
- Allow

- Deny

- Session Closed

### IPS/IDS (Intrustion Prevention System  / Intrusion Detection System)
Command IPS/IDS attributes:

- Monitor
	- Source IP/Port, Dest_IP/Port

- TODO

### Operating System
OS produces a large amount of events, but the most unique are:
- Registry Modification
	- A file i n the registry of the system was edited

- Aduit Log Cleared
	- The system deteced the Audit Log being cleared
	
- Service Installed
	- A new servvice was installed on the system 

- Process Execution
	- The system detected a process being executed
	
### Proxy
Proxy, sometimes called Forward Proxy, generally has three main events that they will produce:

- URL Filtering

- Application Control

- File Download


## Splunk
___

Splunk Technology


*Input > Parsing > Indexing > Search*
Forwarder > TODO > Indexer > TAs (Technology Addons) _TODO < Still working on this_

Everything is a *Knowledge Object* in Splunk Ex. Lists, Saved Searches, etc.

1. Input
	- Data tagged with `source, host, sourcetype`
	- Data stream broken up to 64kb blocks
	- Not individual events yet
	
2. Parsing
	- Breakes stream into induvidual lines
	- Routes the data to specific TAs (_Techonloy Addon_: ) to handle the data block
	
3. Indexing
	- Parsed events are writeen to an index on disk
	
4. Search
	- User interacts to search the parsed and indexed data
	
### Notes

- Whenever you rename fields, it will be refernece from now on as the new name

- Getting to notable events `index=notable`

*Booleans*

- AND

- OR

- NOT

**Cool Commands**

To remake feilds: `|rename <field> as <new_flield>`

To evaluate and run fucntions on fields: `|eval <new or current field> = <funtion>`

Searching through datamodels (Verbose but Slow): `|datamodel <datamodel_name> <datamodel_node> search` < You don't need to enter `<datamodel_node>` for the initial search.

Marco to remove `All_Traffic` from the results: ``|`drop_dm_object_name(All_Traffic)` ``

Macro to find assets: `` |`assets` ``

Macro to find identities: `` |`identities` ``

### Example Searches

**Get a list of sourcetypes**

` |metadata index=* type=sourcetypes `

**Shows all indexes and how many events were recieved for each**

```
|metasearch index=* 
|stats count by index
```

**Search for what sourcetypes are avaible for each index**

```
|metasearch index=* sourcetpye=*
|stats count by index, sourcetype
```

**Subsearch searching for IPs contained in an asset, doing and count, and pulling the IPs back**

- Something to Note: This search is using a command called `appendcols`, this does a subsearch and is not neccessary, refer to the next example to see a smaller and possibly better version.

```
|`assets`
|search category="PCI" ip="*"
|stats count(ip) as Asset_Count by city
|appendcols
   [
    |`assets`
    |search category="PCI"
    |stats values(ip) by city category |search category=pci
   ]
|search Asset_Count>3
|rename city as City Asset_Count as "Asset Count" category as Category values(ip) as "Raw Assets"
```

**Same output as the previous search, but more effeicent**

```
|`assets`
|search category="PCI" ip="*"
|stats dc(ip) as dc_ip values(ip) as ip by city 
|where dc_ip>3
```

**Running `eval` to evaluate the class type for each ip and output the ip and class**

```
index=pan_network
|table src_ip
|eval my_field = if(cidrmatch("192.168.0./16", src_ip), "Class C", "Not Class C")
```

**Using Tstats to search datamodel populated selected fields**

```
|tstats values(All_Traffic. dest_ip) as dest_ip
	from datamodel=Network_Traffic.All_Traffic
	where nodename=All_Traffic.Traffic_By_Action.Allowed_Traffic
	by _time, All_Traffic.scr_ip, span=1h
```

**Using `bin` to batch together specific events based on time**

```
|datamodel Network_Traffic search
|bin _time span=5m
|stats count by _time
```

## QRadar
___



## LogRhythm
___



## ArchSight
___

