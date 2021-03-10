# iDRAC training note
```
start on 2021/02
present date: 2021/03/29
```
--- 
## Referance  
[team file server] - (\\serverip\public\00-Projects\02-DELL\88-Common_ForDELL\IDrac Training Video\iDRAC Training)   
    
[note in github] - (https://github.com/aaarrvhh/SOP_in_fJOB/blob/main/PE/i_D_R_A_C_training_note_202102.md)
    
[iDRAC Training video status sheet] - (https://docs.google.com/spreadsheets/d/arv12C9t6jBy2LmYTBjkPxTO3k8IH2NAuom3YTljcYB38Mg/edit#gid=2059105827)
    
[this document with picture] - (https://docs.google.com/document/d/1tNUqX1nVq2aV3b3tU79nrsWaRE5zmq7Tl-J3lDEvywUarv/edit?usp=sharing)
    
--- 
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

execute `Debugcontrol -l 10 -s 5 -s 10 -o 3` which can be collected for storage logs (RTCEM and MCTP).
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
    



In this video, login to Axon web -> found “iDRAC BDC Automation” by searching in Marketplace → after execute “iDRAC BDC Automation”,  introduce how to configure and start workflow for running the automation test.     
    





in the “start workflow” page. uses needs to select or fill some fields before start.   
    


field “component_name”    
select function like as redfish / racadm  
    


field “email_addr”  
can multi email address and separate by “ , ”    
    
field “input_file”
the config.ini file can be downloaded from the “start workflow” page.   
It shows the configuration parameter for the testing environment. and upload this config.ini file when login Web. 
    
Before executing “start workflow”, it will need to make sure the system will not stop. therefore, check below to disable whose function could make system stop.   
- 進iDRAC web 使用 virtual concole 需要 database license    
- 裡面有提到要進server  去reboot system 然後進F2 BIOS step menu.   
`System BIOS -> Miscellaneous Settings -> F1/F2 Prompt on Error -> set to Disabled`   



- 用talend API tester (chrome extention) 來看lifecycle controller status is Enable via redfish.   
PATCH - `{“Attributes”:{”Lockdown.1.SystemLockdown”:“Disabled”}}`   
    



 
After the automation is finished, the user will receive the mail with a URI link for the result.   



Download the log file from the DUFExcution.   






after start workflow   
The first page will show workflow in here.  
execute console -> 可以看到 command line 正執行到哪.   





Dashboard - 可以看正在excution的workflow status - 也可以click那個時間進入workflow的畫面.






---
## Automation for LC RE - ADC
> reference
> - ODM Overview LCRE Base-NIC-SA.pdf

>>>> the workflow web link can not be entry

LC - ifecycle Controller  
RE - 

Introduce test prerequisites equipment.   

---
## Automation for PI - ADC
> reference
> - ODM Platform Infra and Power training.pdf

PI - Platform Infrastructure   

>>> notes  
>>>> the workflow web link can not be entry  

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



---
# Below is not for presentation. self only.   
---   
## iDRAC Architecture & Syst. Mgmt. (I-band & OOB)+Interface description in Backup
> reference
> iDRAC9 Architecture.pptx

##### iDRAC FW Architecture    

##### iDRAC Interface    


##### iDRAC Boot Process
- A 4MB SPI device holds the secure boot block, UBOOT images along with  persistent storage, LC Logs Etc. 
- A NAND (eMMC) stores the Linux images along with lifecycle controller partitions.



##### iDRAC Networking






---  
## RSA Setup
> reference
> RSA Setup.docx

There is a link in this documentation. this link provide the ova image for “RSA AM server VM with OVA template”
This documentation introduces how to configure the RSA secure and enable SecureID. 

---  
## Quick sync setup
> reference
> iDRAC Quick Sync Feature.docx

the user's mobile can connect to iDRAC via Bluetooth.    
This documentation introduces how to set up the connection.








---   
# END of FILE
---   






