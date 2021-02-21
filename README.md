
# About
Simple scripts to securely update DNS details using the Cloudflare API.  

Differently from other tools, this script follows the least privilege security principle and will allow DNS updates without having to provide a Cloudflare Global API Key. 

Configuration files and the script permissions be easily to whatever level of privilege is required.

Being a simple shell script it is also easy to modify and extend the functionality to your hearth's content. 


# Requirements
This script is meant to be run on any Linux based OS as long as the following dependencies are satisified:
## bash
Bash is an sh-compatible command language interpreter that executes commands read from the standard input or from a file.

About Bash: https://linux.die.net/man/1/bash 

## cURL
cURL is a command-line tool for transferring data specified with URL syntax.

About cURL: https://github.com/curl/curl

## jq
jq is a lightweight and flexible command-line JSON processor.

About jq: https://stedolan.github.io/jq/

## dig
dig (domain information groper) is a flexible tool for interrogating DNS name servers.

About dig: https://linux.die.net/man/1/dig 


# File Locations
Make sure you move the files to the correct locations:

* Config File: /etc/cloudflare.conf
* Shell Script: /usr/sbin/cloudfare_updater

# File Permissions
Make sure you set the permission of your files as following. This will limit access to the script and configuration files that contain your API keys to root user only. 

## File: /usr/sbin/cloudfare_updater
```
Access: (0700/-rwx------)  
Uid: (0/root)   
Gid: (0/root)
```

### How to set the permissions: 
Run the following commands as root user:
```
chown root.root /usr/sbin/cloudfare_updater
chmod 700 /usr/sbin/cloudfare_updater
``` 

## File: /etc/cloudflare.conf
```
Access: (0600/-rw-------)
Uid: (0/root)
Gid: (0/root)
```

### How to set the permissions: 
Run the following commands as root user:
```
chown root.root /etc/cloudflare.conf
chmod 600 /etc/cloudflare.conf
```


# Log File
The script will automatically append the file: ```/var/log/cloudflare-updater.log```

Feel free to adjust the location of the file as you see fit. 


# Configuration
This script uses the CloudFlare API to update DNS records. The values on the configuration file can be referenced directly to the official Cloudflare API Specs.

See Cloudflare documentation here: https://api.cloudflare.com/#dns-records-for-a-zone-update-dns-record

## Obtaining API Key
1. Login to your Cloudflare account 
2. Browse to https://dash.cloudflare.com/profile/api-tokens
3. Click Create API Token
4. Select the 'Edit zone DNS' template
5. Select the zone you want to update in the 'Zone Resources Section'
6. Customize other options as desired.
6. Click Continue to Summary.
7. If you're satisfied with the summary, click 'Create Token'
8. You can now test the token with the shell script provided. Note that running this on your terminal will store your newly created API key on your terminal history in clear text, which is not ideal for security. As an alternative, you can copy this key directly to the configuration file ```API_TOKEN``` variable, and use the cloudflare-updater itself to test the API key.

## Other configuration variables
### ZONE
This identifies the Zone ID to be updated, and is available on the right lower corner of your Cloudflare dashboard.

### RECORD
By having the API KEY and ZONE information on the configuration file, it is possible to call ```cloudflare-updater -l```
The ```-l``` option will list all available records within than zone, including zone id, name and type. 
Use this to select the record ID you'd like to update

### TYPE
Similar to the Zone record ID, the command ```cloudflare-updater -l``` will give the record type for each of the records. 

### NAME
Similar to the Zone record ID, the command ```cloudflare-updater -l``` will give the record name for each of the reacords. 


### TTL
Time to live for DNS record. Default value of 1 is 'automatic'.

### PROXIED
Whether the record is receiving the performance and security benefits of Cloudflare

Default to ```true```


## Crontab
Add a crontab record to make it update your DNS record periodically. 

Example:

```@hourly /usr/sbin/cloudfare_updater```


# I want to Contribute
Feel free to raise issues or submit a pull request with any changes you'd like to see here. 
