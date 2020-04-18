# DNSUpdate

A python script to aid Responder in gathering more hashes even from different VLANs, which by default is not possible with Responder. The scripts does so by updating DNS entries in ADIDNS zones. The script requires a set of valid domain credentials to update the ADIDNS zones.

This could be helpful in the following scenarios:
- when LLMNR and NBNS are disabled | we can point the unresolved records to Responder's IP directly
- when admins are in different VLAN | we can add records to DNS which would allow us to gather hashes bypassing the broadcasting limitation

# Overview

By default Windows ADIDNS (Active Directory Integrated DNS) zones allow any authenticated users to add/ modify/ delete DNS entries. Using this any user account in the AD can add new DNS records. 

- The script can be used with Responder's logs in analyze mode to identify records which have been requested by multiple hosts. These records are likely to be requested by hosts in other VLANs as well. Thus modifying the zones with these records (and pointing to Responder's IP) may allow us to gather hashes from different VLANs.
- The script can also be used to add Wildcard DNS records which makes DNS works just as LLMNR and NBNS (which means all unresolved records would fall down to the wildcard record and thus resolve to Responder's IP).

**Note: Adding Wildcard records may cause disruptions in the network, as a precuationary measure wildcard records should be added to last zones.**

The script right now is not capable of adding WPAD records thus trying that might fail. 

# Installation

- Clone the repository via git clone 
- Install the requirements via pip

# Usage
Run the script with a set of credentials and a unresoloved record to be added to DNS zones, pointing the record to Responder's IP.
```
$ python3 DNSUpdate.py --help
usage: DNSUpdate.py [-h] [-DNS DNS] [-u USER] [-p PASSWORD] [-a ACTION] [-r RECORD] [-d DATA]
                    [-l LOGFILE]

Add/ Remove DNS records for an effective pawning with responder

optional arguments:
  -h, --help            show this help message and exit
  -DNS DNS              IP address of the DNS server/ Domain Controller to connect to
  -u USER, --user USER  Domain\Username for authentication.
  -p PASSWORD, --password PASSWORD
                        Password or LM:NTLM hash, will prompt if not specified
  -a ACTION, --action ACTION
                        ad, rm, or an: add, remove, analyze
  -r RECORD, --record RECORD
                        DNS record name
  -d DATA, --data DATA  The IP address of attacker machine
  -l LOGFILE, --logfile LOGFILE
                        The log file of Responder in analyze mode
```
# Acknowledgements

Thanks to [@Kevin-Robertson](https://github.com/Kevin-Robertson), [@dirkjanm](https://github.com/dirkjanm) and [@mubix](https://github.com/mubix) for their research and code
