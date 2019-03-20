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
