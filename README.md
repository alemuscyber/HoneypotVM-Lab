# Azure HoneypotVM and MS Sentinel Lab
In this project, I created through Azure a vulnerable Win10 x64 VM and deployed it using a virtual network with 10.0.0.0/16 address space. I configured an agent and established a DCR (Data Collection Rule) to collect security logs and forwarding them to Microsoft Sentinel for enhanced security visibility.

![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/64acd06a5158cbd21d3ac85f42bdb9a36911d6e4/Screenshot%202026-07-14%20220649.png)

Here's a walkthrough:

Step 1: Establishing a Virtual Network for my Resource Group in Azure.

Step 2: Created the VM using a Win10 x64 image.

Setting up admin account, giving the machine an attractive name to lure attackers to it, allowing RDP port 3389, assigning the network interface to my virtual network I created in step 1, also established a default subnet being 10.0.0.0/24. I configured a basic NIC Security Group, disabled boot diagnostics.

![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/87718f192f6ca9c19221cd941557be7f5017e5b5/Screenshot%202026-07-15%20172048.png)

Step 3: Modifying the Security Group of the VM

I assigned an inbound rule to allow any for Port, Protocol, Souce, Destination. This makes our VM reachable and vulnerable over the virtual network. To ensure reachability I sent ICMP Pings to the VMs IP Address and got succesfull replies.

![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/3b995e07f23c23760adaaba751ead4a2c66ffe97/Screenshot%202026-07-15%20172344.png)

Step 4: Accesing the VM and disabling all FW policies

This makes our VM highly vulnerable and attractive for potential attackers. For this lab, I turned off the FW for all profiles (Domain, Private, Public).

![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/645ef3c2e06db9a510fa8c6070264d1ea9eebb4c/VMfw_turnoff.png)

Step 5: Creating our Log Analytics Workspace

The value of this lab, asides from the honeypot itself, is avoiding to access the VM to review the internal security logs. These logs can actually be forwarded and centralized in Azures Log Analytics Workplace. For this, I installed Windows Security Events solution which uses both AMA (Azure Monitor Agent) and a Legacy Agent. After installing the solutions I established the connection from the VM to the workspace, this can only happen by creating a DCR (Data Collection Rule) and link it to the resource group. I configured the rule to stream *all security events*.  Once this is done, the connection is now succesful. You can verify the stream and storage of security events using KQL in the Log Analytics Workspace. Here's an example of a query:

![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/e6af539aaec3b7b6933321defca89332840d9594/Screenshot%202026-07-15%20180738.png)
*Note: It already queries existing security events because I took this screenshot about an hour after. You can expect to get your security events during this time as well, if you do it to early you'll probably be querying system noise.*

Step 6: Is this a SIEM yet?

No, not yet. We can't call a Log Repository a SIEM by simply collecting and querying raw data. So I turned this data into valuable information. Also, a SIEMs purpose is to enhance visibility, a single pane of glass, of security events that have been previously filtered to reduce inaccuracy and avoid noise. I created a Microsoft Sentinel Workspace and linked it to my Log Analytics Workspace. Then, to bring actual value, I uploaded a D.B ("geoip-summarized.csv") of more than 50k records in order to correlate the attackers IP addresses of my security events to cityname and countryname. Here's an example of a query to validate succesful correlation:

![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/08396fe1c2d38a6ed8c46987e2fc3e15d8f4e581/Screenshot%202026-07-15%20174658.png)

Step 7: Building my SIEM with MS Sentinel

As I said, the purpose of a SIEM is to enhance security visibility. This is done by gathering valuable insights, creating graphs/charts/kpis/etc, and centralizing them in a single pane of glass (dashboard).

*My final product looks like this:*

![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/64acd06a5158cbd21d3ac85f42bdb9a36911d6e4/Screenshot%202026-07-14%20220649.png)
![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/a826972951f5ce2433515dfc922469f065a0e476/Screenshot%202026-07-14%20220758.png)
![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/5d8d41f7e3eb99ec165484ad439b71dde107419e/Screenshot%202026-07-14%20220918.png)
