ansible-joyent role
=====================

A role to provision and manage machines in joyents SDC 

Variables
---------

```
## ********** Machine parameters **********
mach_state              : "present"  # ["absent", "present", "running", "stopped"]
## Image and flavor https://www.joyent.com/public-cloud/pricing
mach_image              : "sdc:canonical:ubuntu-certified-12.04:20140929"
mach_flavor             : "g3-standard-1-kvm"
## Datacenters https://www.joyent.com/public-cloud/data-centers
mach_dc                 : "eu-ams-1"
## Comma seperated network UIDs
mach_networks           : "1e7bb0e1-25a9-43b6-bb19-f79ae9540b39"
## Credentials if the following is not defined will fall back to bash environment variables 
#joyent_key_name        : "mykey"          # if not defined will fall back env var $JOYENT_USERNAME
#joyent_user_name       : "myuser"         # if not defined will fall back env var $JOYENT_KEYNAME
## If your using encrypted keys ssh-agent must be running
#joyent_private_key     : "~/.ssh/id_rsa"  # if not defined will fall back ~/.ssh/id_rsa 

            
## ************ IP matching rules ********
## Network:   10.224.24.0/22 (Class A)
## HostMin = 10.224.24.1 and HostMax = 10.224.27.254
## Rules are expresed in list:
##ip_match_rules : 
##                - regx      : "10.224.2[4-7].*"
##                  reg_match : false
##                - regx      : "10.224.2[4-7].*"
##                  reg_match : true
##
## The above 2 rules are expressed as following
##  First rule match any ip that "is not" 10.224.2[4-7].* 
##  if first rule fails second rule say match any ip that is 10.224.2[4-7].* 
##  The logic is :
##     - we try to match any IP that is not in our private vpn network 
##     - if that rule fails then we use our private vpn IP
joyent_match_rules :
                - regx      : "10.224.2[4-7].*"
                  reg_match : False

                - regx      : "10.224.2[4-7].*"
                  reg_match : true

## ************ Inital user for SDC ********
joyent_inital_user : "ubuntu"  # over ride with 'root' for smartos machines

## ************ Extract machine Name ********
## Define what part of fqdn is the hostname lets say "postgresql00.de.prod.example.com"
## Option (A) We can use subdomain level from left
##     i.e. use 'joyent_hostname_sub_level' and set
##       value "1"  result is "postgresql00"
##       value "3"  result is "postgresql00.de.prod"
##  To enable override somewhere in hostvar or inventory file
##
## Option (B) We define top level domain to exclude
##     i.e.use 'joyent_hostname_sub_replace'
##       value "de.prod.example.com"  result is "postgresql00"
##       value "example.com" result is "postgresql00.de.prod"
##  To enable override somewhere in hostvar or inventory file
## Option (C) We do nothing and in just user value in inventory_hostname thats the default
##     i.e.
joyent_hostname_sub_level    :  false
joyent_hostname_sub_replace  :  false

## ************ Should wait for machine state to change ********
joyent_fire_forget    : "false"

## ************ SDC tags ********
## by default joyent module adds two tags
##  1. provisioner: your username
##  2. provisioner_ver: the module verision
## You can extend tags with your own tags
## i.e.
# joyent_tags :
#               env   : "dev"
#               group : "postgresql"
```

Requirements
------------
* tested on Ubuntu 12.04 but should work with most *nix 
* python smartdc module install 

Comments
--------
As best practice so far We define our variables in groups. i.e. a database group would have specific  image/falvor and so ...

