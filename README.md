# portfolio
This repo contains files used to show my before, during and after journey of creating a ckoud environment in the cloud and then securing it. I exposed it to the public internet and allowed all traffic to the virtual machines, key storage, active directory, and any other resources.

I then went through the process of directing all logs from VMs and their network security groups to a single LAW (Log Analytics Workspace). Then Microsoft Defender for Cloud was activated, enabling continuous export of logs into the LAW.Microsoft Sentinel was added next, and custom workbooks were created allowing us to plot on the a map where in the brute force attacks and other suspisious activity were coming from. This is where custom KQL queries were set up as alerts to create Security Alerts/Incidents.

After initial setup (including several Powershell scripts and manual "attacks" used to trigger and test custom alerts), the insecure environment was ran for 48 hours, and metrics were recorded via KQL queries. Incidents were dealt with in a manner just like in a normally operating SOC, recording the IPs, whether they were involved in other incidents and any other metrics and the incidents were marked appropriately (true/false positive, benign, suspicious, expected, etc). 

After investigating the incidents, we already knew the cause, our environment was completely insecure. NSGs were locked down, firewalls enabled and an additional NSG added to including services that needed to communicate with each other, but didn't need to communicate with the public internet. And while this setup was relatively simple, the techniques used could be included in any cloud environment where multiple virtual networks needed to, machines that need to be public facing are connected to those that don't and many other use cases.