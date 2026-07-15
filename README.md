# HoneypotVM-Lab
In this project, I created through Azure a vulnerable Win10 x64 VM and deployed it using a virtual network with 10.0.0.0/16 address space. I configured an agent and established a DCR (Data Collection Rule) to collect security logs and forwarding them to Microsoft Sentinel for enhanced security visibility.

Here's a walkthrough:

Step 1: Created a Virtual Network for my Resource Group in Azure.

Step 2: Created the VM using a Win10 x64 image.
Setting up admin account, giving the machine an attractive name to lure attackers to it, allowing RDP port 3389, assigning the network interface to my virtual network I created in step 1, also established a default subnet being 10.0.0.0/24. I configured a basic NIC Security Group, disabled boot diagnostics.

Step 3: Modifying the Security Group of the VM
I assigned an inbound rule to allow any for Port, Protocol, Souce, Destination. This makes our VM reachable and vulnerable over the virtual network.


![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/64acd06a5158cbd21d3ac85f42bdb9a36911d6e4/Screenshot%202026-07-14%20220649.png)

![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/a826972951f5ce2433515dfc922469f065a0e476/Screenshot%202026-07-14%20220758.png)

![Image Alt](https://github.com/alemuscyber/HoneypotVM/blob/5d8d41f7e3eb99ec165484ad439b71dde107419e/Screenshot%202026-07-14%20220918.png)
