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
-- memory: check/analyze for leaks.  

>> notes:  
>>> throughput / network / load-bearing


- Stress Test  
```
-- iDRAC stressed from multiple paths to ensure:  
  -- Robustness.  
  -- Availability and reliability.  
-- Stress executed for:  
  -- All iDRAC interfaces (WSMAN, GUI/REST, RACADM, REDFISH, IPMI, SNMP)  
  -- iDRAC firmware update.  
  -- iDRAC Login.  
  -- Server reboot and powercycle.  
  -- iDRAC reboot.  
  -- Network initialization.  
  -- File export.  
  -- Alert generation (LCLog, SNMP Trap, WSEvent, Redfish event)  
  -- Serial/SOL stress.  
  -- SCP stress.  
  -- RAID create/delete stress.  
  -- Stress from all interfaces simultaneously.  
  -- Long duration stress.  
```

- Scalability test :   
>>> notes  
>>>> Vertical Scaling : single blade v.s. rack  
>>>> Horizontal Scaling : more rack or more single blade on the same network/ power environment. added more users in a system. 
>>>>

- Longevity Test : leave the system / function in an idle state and keep it alive through 2~3 weeks.  

---
## automation for IDRAC BDC Automation Training(Redfish/Wsman/Racadm/RTCEM)
> reference  
> - ODM Traning PPT- RTCEM.pptx

#### Managing  storage devices
RTCEM managed storage controllers, NVMe, BOSS, HBA, PERC.    

>>> notes  
>>>
>>>> Real-Time Configuration Enablement Management
>>>> RT-CEM is a software module that handles individual parameters and real-time notifications from a device, such as a Fibre Channel HBA or NIC.   
>>>> RT-CEM runs on the iDRAC7 and uses an i2C bus to communicate with the device, and then directly writes to the HII populator that writes data to the Data Manager.  
>>>
>>>> Boot Optimized Server Storage (BOSS)  :    
>>>> Dell has developed its BOSS for the line of servers called Dell PowerEdge. The idea behind it was to create a hardware RAID, the RAID 1 configuration, on a budget.    
>>>> RAID 1 is a very simple RAID configuration which makes a complete mirror of a drive. It does not increase performance, but it serves as a backup.    
>>>> Dell provides this mirroring for the drive where the OS is installed. If something happens to the OS drive, there is a backup that continues operating without any problems.    
>>>> This comes from the consumers, who wanted a separate hardware controller for that purpose.    
>>>> This is a common desire of companies that implement SDS – Software-Defined Storage and those with HCI – Hyper-Converged Infrastructure. It is good for application data.    

- Operations:
```
Controller operations : Reset, clear/import foreign config, change controller mode, 
    change controller attributes, create/modify/delete security key, 
    discard preserved cache, set boot VD, start/stop patrol read, unlock secured foreign drives    

VD operations : Create VD, Delete VD, RLM, OCE, Fast/Full Init, Cancel Init, Cancel BGI,
    Check consistency, Cancel Check consistency, Rename VD, assign DHS, 
    encrypt VD, change VD cache policies,    

PD operations: assign GHS, convert to RAID/non RAID, rebuild, cancel rebuild,
    offline/online, cryptographic erase, replace PD    

Enclosure operations: Set Asset Tag, Set Asset Name    
```
>> notes:
>>> VD (Virtual Disk), PD (Pyshical Disk)

- Key callouts
```
-- 12GBps SAS does not support multipath, so ensure only 1 cable connected from controller to enclosure.
-- PERC external controllers supports redundant path.
-- H3XX does not have battery. It also does not support RAID 6 and 60.
-- Any ready disk inserted to backplane connected to H330 will be detected as NonRAID. For other controllers, the state will be Ready.
-- PERC 10 introduced the concept of profiles. There are 2 profiles: PD64 and PD240. H740 supports only the PD64 profile. 
    H745P MX supports only the PD240 profile. The H840 supports both PD64 and PD240. 
    PD64 profile means a max of 64 PD are supported and 64 VD can be created. 
    PD240 means a maximum of 240 PDs are supported and 240 VDs can be created
-- 16 VDs can be created on 1 PD. 
-- PERC 9 has RAID and HBA modes. PERC 10 has RAID and eHBA modes. PERC 11 has no mode settings. 
    It has autoconfig non-RAID which can be configured from HII.
-- All PERCs have autoconfig RAID 0 option from HII.
```

- log required
```
-- execute `Debugcontrol -l 10 -s 5 -s 10 -o 3` which can be collected for storage logs (RTCEM and MCTP).    
-- press `F12 key` for Chrome browser when meet the WEB GUI relative issues. 
    get the HAR `HTTP Archive format` file’s content to DF.   
-- PERC log: there are two interfaces that are TTY and I2C. it can get the logs through both interfaces.    
```
- reference documents    
[MCTP] (https://www.dmtf.org/sites/default/files/standards/documents/DSP0237_1.1.0.pdf)    
[PERC 10 UG]: (https://topics-cdn.dell.com/pdf/poweredge-rc-h740p_users-guide_en-us.pdf)    
[iDRAC user guide]: (https://topics-cdn.dell.com/pdf/idrachalo_en-us.pdf)    

#### Managing  network devices

- Types of form factors:  PCIe, LOM, OCP3, Fab A/B Mezz, Fab C Mezz

- log required
```
Screen shot of BIOS > F2> Device Settings page list all the ports
iDRAC web GUI -> iDRAC Settings -> Network Settings -> NIC Selection drop down menu options.
 
Output of below command from iDRAC shell
cfgutil command=getattr key="iDRAC.Embedded.1#NIC.1#Selectionfqdd"
cfgutil command=getattr key="iDRAC.Embedded.1#NIC.1#NICPresenceMask"
cat /tmp/NetworkPreDiscovery.txt
cat /tmp/NetworkStage2Discovery.txt
cat /sys/bus/platform/drivers/ncsi_protocol/ncsi_device_info
PCILibTest -p
 
Please provide below logs
debugcontrol -s NICRTCEM -l 10 -r 10000
touch /tmp/HIIpop_log
 
WAIT FOR 3 mins and share below 3 files
cat /var/log/idraclogs, cat /tmp/wtpacket.log, cat /tmp/rhtpacket.log
 
debugcontrol -d
rm -rf /tmp/HIIpop_log, rm -rf /tmp/wtpacket.log, rm -rf /tmp/rhtpacket.log
```
- reference documents
[Understanding Dell network
adapters and their
relationship to iDRAC with
Lifecycle Controller]       (https://downloads.dell.com/solutions/dell-management-solution-resources/Dell%20Network%20Adapters%20and%20DRAC%20LC_Dec%202016.pdf)    
[Understanding Dell EMC network adapters and
their relationship to iDRAC with Lifecycle
Controller]      (https://downloads.dell.com/manuals/all-products/esuprt_software/esuprt_it_ops_datcentr_mgmt/dell-management-solution-resources_white-papers_en-us.pdf)    

>>> notes
>>>> 這些文件是列出網路卡跟每個platform的support.    

#### Virtual Address Management

- Aux power        : Fab A/B Mezz , LOM , OCP3
- non-Aux power : Fab C Mezz , PCIe

- reference documents
[Optimizing I/O Identity and Applying Persistence
Policy on Network and Fibre Channel Adapters]   (https://downloads.dell.com/solutions/general-solution-resources/White%20Papers/13G_IO_IOOpt_Persistence_Reviewed_WhitePaper_May2015.pdf)      

>>> notes
>>>> (https://www.dell.com/support/manuals/zh-tw/idrac9-lifecycle-controller-v3.3-series/idrac_3.30.30.30_ug/dynamic-configuration-of-virtual-addresses,-initiator,-and-storage-target-settings?guid=guid-7c85f407-4349-4f8a-b961-3537ad7c1b60&lang=en-us)    
>>>> dell的user guide之類的網頁.也在說VAM相關的. 看起來比 power pointer的內容還要多

#### PCIe VDM
```
-- iDRAC inventory of storage and network devices is via I2C channel which is of low bandwidth   
-- PCIe VDM enables iDRAC to use PCIe channel to inventory devices   
-- Attribute in iDRAC which can be enabled/disabled (#iDRAC.PCIeVDMEnable)    
-- When attribute is enabled, all devices which do not support PCIe VDM continue t use I2C channel.   
-- Few network vendors already support PCIe VDM.
```

>>> notes
>>>> MCTP PCIe Vendor Defined Message (VDM) Transport Binding Protocol
##### figure: VDM vendor defined Message

>>>
>>>> [Overview of the NVMe Management Interface Specification]   
>>>> (https://www.snia.org/sites/default/files/SDCIndia/2016/Presentations/NVMe-MI%20SDC%20India.pdf)    
>>>> Storage Networking Industry Association    
>>>> The SNIA is a non-profit global organization dedicated to developing standards and education programs to advance storage and information technology.   
>>>




---
## Automation for iDRAC -ADC
> reference  
> - iDRAC automation-2020100801.mp4
> - iDRAC automation-2020100802.mp4
> - iDRAC automation-2020100803.mp4

`login.axon.dell.labs.net/login`  -> `workflow.axon.dell.labs.net`
Axon -> Marketplace  
    
##### figure: 



In this video, login to Axon web -> found “iDRAC BDC Automation” by searching in Marketplace → after execute “iDRAC BDC Automation”,  introduce how to configure and start workflow for running the automation test.     
##### figure:   how to find iDRACBDCAutomation in Marketplace





in the “start workflow” page. user needs to select or fill some fields before start.   
    
##### figure:  iDRACBDCAutomation’s entry page in Axon. 


field “component_name”    
select function like as redfish / racadm  
##### figure:   


field “email_addr”  
can multi email address and separate by “ , ”    
    
field “input_file”
the config.ini file can be downloaded from the “start workflow” page.   
It shows the configuration parameter for the testing environment. and upload this config.ini file when login Web. 
    
Before executing “start workflow”, it will need to make sure the system will not stop. therefore, check below to disable whose function could make system stop.   
- 進iDRAC web 使用 virtual concole 需要 datacenter license    
- 裡面有提到要進server  去reboot system 然後進F2 BIOS step menu.   
`System BIOS -> Miscellaneous Settings -> F1/F2 Prompt on Error -> set to Disabled`   
##### figure: Disable “F1/F2 Prompt on Error “


- 用talend API tester (chrome extention) 來看lifecycle controller status is Enable via redfish.   
PATCH - `{“Attributes”:{”Lockdown.1.SystemLockdown”:“Disabled”}}`   
    
##### figure: talend API tester   



 
After the automation is finished, the user will receive the mail with a URI link for the result.   
##### figure: dell’s mail


Download the log file from the DUFExcution.   
##### figure: DUFExcution





after start workflow   
The first page will show workflow in here.  
execute console -> 可以看到 command line 正執行到哪.   
##### figure:  find out flow an status 




Dashboard - 可以看正在excution的workflow status - 也可以click那個時間進入workflow的畫面.
##### figure:  dashboard






---
## Automation for LC RE - ADC
> reference
> - ODM Overview LCRE Base-NIC-SA.pdf

>>>> the workflow web link can not be entry

LC - ifecycle Controller  
RE - 

>>> notes  
>>>> Introduce test prerequisites equipment.   
>>>
>>>> Remote Enablement      
>>>> RE is a software module that runs on the iDRAC7 to collect information from various devices, such as a Fibre Channel HBA or NIC on a server.    
>>>> RE communicates with the Unified Extensible Firmware Interface (UEFI) on the PCI bus to get information about these devices, and then populates the HII populator that writes data to the Data Manager.   
>>>> The RE module uses memory partitions on the iDRAC7 to save and store information before passing it on to the Data Manager.   
>>>
>>>> [Fibre Channel Host Bus Adapters for Dell PowerEdge Servers]   
>>>> (http://legislacao.sema.ma.gov.br/arquivos/1481047547600.pdf)   
>>>>  在這份裡面有說一些 dell 的 term.

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

##### figure: iDRAC FW Architecture    

##### figure: iDRAC Interface    


##### figure: iDRAC Boot Process
- A 4MB SPI device holds the secure boot block, UBOOT images along with  persistent storage, LC Logs Etc. 
- A NAND (eMMC) stores the Linux images along with lifecycle controller partitions.



##### figure: iDRAC Networking






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
##### figure: iDRAC app in mobile.







---   
# END of FILE
---   






