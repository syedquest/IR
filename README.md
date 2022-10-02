# Windows Event Log Analysis
Notes for performing evtx log analysis

## Why analyze Windows Event Logs:
- Create timeline of events such as logon event, system/application error, and system modification

## Location
\Windows\System32\winevt\Logs

## Event Viewer Tools
- Event viewer
- Event log explorer

## Main event logs
- **Security.evtx**
  - Records log related to account management, account logon, share access events
  - Notable event IDs:
    - EID 4624
      - Logon to the system events
      - Take note of the account name, account domain, workstation name, source network address, and logon type
      - There are several logon types that explains the how the user access the system
        - Logon type 2 - Interactive logon. It is a logon through the keyboard/screan of the system. Could also be through remote access tools such as VNC. 
        - Logon type 3 - Network logon. Access from other location in the network such as shared folder access.
        - Logon type 4 - Batch. Used by batch server. Usually, it is the schedule task. 
        - Logon type 5 - Service that has been configured to run as a user account.
        - Logon type 7 - Unlock. User logon on to the machine after it was locked.
        - Logon type 10 - RemoteInteractive logon. It is logged when user access through Terminal Services or RDP
