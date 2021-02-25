# iDRAC training note

## NFT(Longevity, Scalability, Performance, Usability Test)
> reference
> - NFT20201021.mp4 - presentation base on “NFT Training.pptx”
> - NFT Training.pptx

NON-FUNCTIONAL TESTING is defined as a type of Software testing to check non-functional aspects (performance, stress, Scalability test, longevity, etc) of a software application.  
- Performance Test  
-- response time:   
-- memory: check/analyze for leads.  

>> notes:  
>>> throughput / network / load-bearing


- Stress Test  
-- iDRAC stressed from multiple paths to ensure:  
--- Robustness.  
--- Availability and reliability.  
-- Stress executed for:  
--- All iDRAC interfaces (WSMAN, GUI/REST, RACADM, REDFISH, IPMI, SNMP)  
--- iDRAC firmware update.  
--- iDRAC Login.  
--- Server reboot and powercycle.  
--- iDRAC reboot.  
--- Network initialization.  
--- File export.  
--- Alert generation (LCLog, SNMP Trap, WSEvent, Redfish event)  
--- Serial/SOL stress.  
--- SCP stress.  
--- RAID create/delete stress.  
--- Stress from all interfaces simultaneously.  
--- Long duration stress.  

- Scalability test :   
>> notes:  
>>> Vertical Scaling : single blade v.s. rack  
>>> Horizontal Scaling : more rack or more single blade on the same network/ power environment. added more users in a system. 

- Longevity Test : leave the system / function in an idle state and keep it alive through 2~3 weeks.  

---
## automation for IDRAC BDC Automation Training(Redfish/Wsman/Racadm/RTCEM)
> reference  
> - ODM Traning PPT- RTCEM.pptx

RTCEM managed storage controllers, NVMe, BOSS.  

execute `Debugcontrol -l 10 -s 5 -s 10 -o 3` which can be collected for storeage logs (RTCEM and MCTP).
press `F12 key` for Chrome browser when meet the WEB GUI relative issues. get the HAR `HTTP Archive format` file’s content to DF.
PERC log: there are two interfaces that are TTY and I2C. it can get the logs through both interfaces. 

>> notes:  
>>> there are more debug commands in this document.   


---
## Automation for iDRAC -ADC
> reference  
> - iDRAC automation-2020100801.mp4
> - iDRAC automation-2020100802.mp4
> - iDRAC automation-2020100803.mp4

`login.axon.dell.labs.net/login`  -> `workflow.axon.dell.labs.net`
Axon - Marketplace  


---
## Automation for LC RE - ADC
> reference
> - ODM Overview LCRE Base-NIC-SA.pdf

>>>> the web link can not be entry


---
## Automation for PI - ADC
> reference
> - ODM Platform Infra and Power training.pdf

>>> notes  
>>>> the web link can not be entry  

##### Introduce the equipment and environment..  
- PDU -power distribution units  
- Host OS  
-- tool should be provided by customer subject matter expert
-- tool some are under NDA, which needs to be coordinated
- Test length controls
- Debug/special case options

##### setting up a windows system for platform infrastructure  
- install win 2019  
- enable remote desktop access  
- enable WMI  
-- command line: `netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`    
-- `winrm quickconfig`   
- install stress test   
- enable inbound connections in windows firewall   
- install openssh  
- ensure host OS time and time are set properly  
- on windows 10+  
-- execute `gpedit.msc` : Change Computer configuration\administrative templates\network\Lanman Workstation "Enable insecure guest logons" to "Enabled"   

##### setting up a Linux system
- ensure SSH is enabled from outside
- install stress utility

##### example failures:
- script should handle error cases cleanly.   



---
## Automation for Telemetry






