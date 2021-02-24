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


---
## Automation for iDRAC -ADC
> reference  
> - iDRAC automation-2020100801.mp4
> - iDRAC automation-2020100802.mp4
> - iDRAC automation-2020100803.mp4

---
## Automation for LC RE - ADC
> reference
> - ODM Overview LCRE Base-NIC-SA.pdf

---
## Automation for PI - ADC
> reference
> - ODM Platform Infra and Power training.pdf

---
## Automation for Telemetry





