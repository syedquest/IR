# Windows Event Log Analysis
Notes for performing evtx log analysis

## Why analyze Windows Event Logs:
- Create timeline of events such as logon event, system/application error, and system modification

## Location
\Windows\System32\winevt\Logs

## Event Viewer/Parser Tools
- Event viewer
- Event log explorer
- Powershell
- EvtxParser
- EvtxECMD (https://github.com/EricZimmerman/evtx)

## Main event logs
- **Security.evtx**
  - Records log related to account management, account logon, share access events
  - Notable event IDs:

#### Sessions logon events
    - Event ID 4624
      - Logon to the system events
      - Take note of the account name, account domain, workstation name, source network address, and logon type
      - There are several logon types that explains the how the user access the system
        - Logon type 2 - Interactive logon. It is a logon through the keyboard/screan of the system. Could also be through remote access tools such as VNC. 
        - Logon type 3 - Network logon. Access from other location in the network such as shared folder access.
        - Logon type 4 - Batch. Used by batch server. Usually, it is the schedule task. 
        - Logon type 5 - Service that has been configured to run as a user account.
        - Logon type 7 - Unlock. User logon on to the machine after it was locked.
        - Logon type 10 - RemoteInteractive logon. It is logged when user access through Terminal Services or RDP
    - Event ID 4625
      - Failed logon events. If there are multiple entries with this event ID, it might indicate a password spraying attack.
      - The failure information section will have the codes for the failed logon events and the reason for the failure.
    - Event ID 4634 / Event ID 4647
      - User log off events. Can be used to tie the EID 4624 event with the log off evnet 
    - Event ID 4648
      - A logon was attempted using explicit credential
      - An example of this event is the admin logs to a machine and access a shared drive using a different credential.
    
#### Account management
    - Event ID 4720
      - A user account was created
    - Event ID 4723
      - An attempt was made to change an account's password
      - Logged when the user attempted to change own password. Would also log EID 4738 (A user account was changed)
    - Event ID 4728
      - A member was added to a security-enabled global group
      - This event can be seen in domain controllers.  
    - Event ID 4732
      - A member was added to a security-enabled local group
      - If the computer name is the same as the domain name, it is a SAM group
    
#### Kerberos Authentication     
    - Event ID 4768 - TGT Granted
    - Event ID 4769
      - It is generated when there is a service ticket request
      - If the request failed, it will populate the failure code field or Event ID 4771 will be recorded.
    - Event ID 4771 - Authentication failed
    - Event ID 4778
      - A session is reconnected event occured. It might happen when the session is reconnected through RDP. Check the network section the the remote host information.
      
#### Event log manipulation
    - Event ID 1102
      - Audit log cleared
- **System.evtx**
  - System event logs records event triggered by the OS such as system/hardware changes, drivers etc. 
  - Below are the noteworthy event IDs
    - Event ID 6005
      - The event log service was started.
      - It occurs when a service is manually started or when the system rebooted.
    - Event ID 6006
      - The event log service was stopped.
      - The event ID can be seen during system shutdown/restart but if it might indicate a malicious event if it occurs several times.
    - Event ID 7034
      - A service terminated successfully. Check the description to see the name of the services.
    - Event ID 7036
      - A service was stopped or started.
    - Event ID 7045
      - A service was installed by the system. 
      - Useful to see if there was a service created on the system.

- **Microsoft-Windows-TaskScheduler%4Operational.evtx (Schedule task log)**
  - Records activity related to schedule task on the machine. Here are the event ID for schedule task log
    - Event ID 106
      - Scheduled task created. When a user created a schedule task, this event ID will be recorded with the name of the user who created the task.
    - Event ID 141
      - Scheduled task deleted. The event ID will have the user and what task that has been deleted.
    - Event ID 200
      - Scheduled task executed. This event ID can be tied with EID 106 to see the user who created the task. 
    - Event ID 201
      - Scheduled task completed. After EID 200 is logged, this event ID might comes next.         
