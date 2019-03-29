# Spe·le·ol·o·gy

## Table of Content

## Walk Thurs

### Searching and Sorting Assets

Description: For this search, we will be looking at the "assets" table and finding all "ip" assets per city. Additionally, we will be excluding any city that has less than 4 ip realted to it.

**Final Search:**
```
|`assets`
|search ip="*"
|stats dc(ip) as dc_ip values(ip) as ip by city 
|where dc_ip>3
```

1. First we can start by using the `assets` macro to print the data in the "assets" lookup table. We can run the macro by starting with a `|` (Pipe).

    Command: `` |`assets` ``

    The output now shows us a tabular view of assets in the "assets" lookup table, as well as any other fields related to the assets.

2. We don't need all of the information presented in the table currently. Also, notice that not all of the entries contain data for the "ip" field. We are going to only pull out entries for assets that contain IPs associated with them.

    Command: 
    ```
    |`assets`
    |search ip="*"
    ```
    
    We can see that now all of the entries contain values for the "ip" field. The `*` (Wildcard) in the command tells Splunk to only show entries that have values in any field specified.

## Rename

This string can be used to rename a field as something else. This is a simple way to do it, many Splunk commands allow you to rename fields within the command itself.

`|rename <field> as <new_flield>`

## Eval

TODO

## Macros

### drop_dm_object_name

This macro allows you to drop the Datamodel object name from the field title.

``|`drop_dm_object_name(All_Traffic)` ``

### assets

Macro used to find assets. _Note: Information in the assests macro may be manually enters and could be out dated. Do not reply completely on the information that this provides._

`` |`assets` ``

### identities

Populates availible identities that are in your instance.

`` |`identities` ``

## Commands

### Stats

Allows you to appregate statictical data from logs.

`| stats [function](field) as [new_name] by [sort_by_field]`

Example: `|stats values(dest_ip) as dest_ip by src_ip`

### Tstats

Tstats is used to conduct "statistical queries" on indexed fields. These indexed fields can be from data models. This command makes searching through large amounts of data extremly fast. 

```
|tstas [function](field) as [new_field_name]
    from datamodel=[]
    where [field]=[value]
    by [field], [optional_field], [optional span "span=1h"]
```

Example:

```
|tstats values(All_Traffic. dest_ip) as dest_ip
    from datamodel=Network_Traffic.All_Traffic
    where nodename=All_Traffic.Traffic_By_Action.Allowed_Traffic
    by _time, All_Traffic.scr_ip, span=1h
```
