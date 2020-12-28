# Festive Calendar 2020 - Hands on lab

### Lab 05a - Implement Intersite Connectivity

**Lab scenario**

Contoso has its datacenters in Boston, New York, and Seattle offices connected via a mesh wide-area network links, with full connectivity between them. You need to implement a lab environment that will reflect the topology of the Contoso's on-premises networks and verify its functionality. 

**Objectives**

In this lab, you will:

+ Task 1: Provision the lab environment
+ Task 2: Configure local and global virtual network peering
+ Task 3: Test intersite connectivity 

##### Task 1: Provision the lab environment

1. The lab environment has already been provisioned for you.

1. Login to the Azure Portal at **[paste-cs-email](urn:it:godeploy:lab-guide:action-link:paste-cs-email)** with the password **[paste-cs-password](urn:it:godeploy:lab-guide:action-link:paste-cs-password)**

 ```cli
https://portal.azure.com
 ```
> **Note**: Wait for the deployments to complete before proceeding tothe next task. This should take about 5-15 minutes. To verify the status of the deployments, you can examine the properties of the resource groups you created in this task.

##### Task 2: Configure local and global virtual network peering

In this task, you will configure local and global peering between the virtual networks you deployed in the previous tasks.

1. In the Azure portal, search for and select **Virtual networks**.

1. Review the virtual networks you created in the previous task and verify that the first two are located in the same Azure region and the third one in a different Azure region. 

> **Note**: The template used for deployment creates three virtual networks and ensures that the IP address ranges of the three virtual networks do not overlap.


1. In the list of virtual networks, click **az104-05-vnet0**.

1. On the **az104-05-vnet0** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Add a peering with the following settings (leave others with their default values):

    | Setting | Value|
    | --- | --- |
    | Name of the peering from az104-05-vnet0 to remote virtual network | **az104-05-vnet0_to_az104-05-vnet1** |
    | Virtual network deployment model | **Resource manager** |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **az104-05-vnet1 (az104-05-rg1)** |
    | Name of the peering from az104-05-vnet1 to az104-05-vnet0 | **az104-05-vnet1_to_az104-05-vnet0** |
    | Allow virtual network access from az104-05-vnet0 to az104-05-vnet1 | **Enabled** |
    | Allow virtual network access from az104-05-vnet1 to az104-05-vnet0 | **Enabled** |
    | Allow forwarded traffic from az104-05-vnet1 to az104-05-vnet0 | **Disabled** |
    | Allow forwarded traffic from az104-05-vnet0 to az104-05-vnet1 | **Disabled** |

> **Note**: This step establishes two local peerings - one from az104-05-vnet0 to az104-05-vnet1 and the other from az104-05-vnet1 to az104-05-vnet0.


1. On the **az104-05-vnet0** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Add a peering with the following settings (leave others with their default values):

    | Setting | Value|
    | --- | --- |
    | Name of the peering from az104-05-vnet0 to remote virtual network | **az104-05-vnet0_to_az104-05-vnet2** |
    | Virtual network deployment model | **Resource manager** |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **az104-05-vnet2 (az104-05-rg2)** |
    | Name of the peering from az104-05-vnet2 to az104-05-vnet0 | **az104-05-vnet2_to_az104-05-vnet0** |
    | Allow virtual network access from az104-05-vnet0 to az104-05-vnet2 | **Enabled** |
    | Allow virtual network access from az104-05-vnet2 to az104-05-vnet0 | **Enabled** |
    | Allow forwarded traffic from az104-05-vnet2 to az104-05-vnet0 | **Disabled** |
    | Allow forwarded traffic from az104-05-vnet0 to az104-05-vnet2 | **Disabled** |

> **Note**: This step establishes two global peerings - one from az104-05-vnet0 to az104-05-vnet2 and the other from az104-05-vnet2 to az104-05-vnet0.

 
9. Navigate back to the **Virtual networks** blade and, in the list of virtual networks, click **az104-05-vnet1**.

1. On the **az104-05-vnet1** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Add a peering with the following settings (leave others with their default values):

    | Setting | Value|
    | --- | --- |
    | Name of the peering from az104-05-vnet1 to remote virtual network | **az104-05-vnet1_to_az104-05-vnet2** |
    | Virtual network deployment model | **Resource manager** |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **az104-05-vnet2 (az104-05-rg2)** |
    | Name of the peering from az104-05-vnet2 to az104-05-vnet1 | **az104-05-vnet2_to_az104-05-vnet1** |
    | Allow virtual network access from az104-05-vnet1 to az104-05-vnet2 | **Enabled** |
    | Allow virtual network access from az104-05-vnet2 to az104-05-vnet1 | **Enabled** |
    | Allow forwarded traffic from az104-05-vnet2 to az104-05-vnet1 | **Disabled** |
    | Allow forwarded traffic from az104-05-vnet1 to az104-05-vnet2 | **Disabled** |

 > **Note**: This step establishes two global peerings - one from az104-05-vnet1 to az104-05-vnet2 and the other from az104-05-vnet2 to az104-05-vnet1.


##### Task 3: Test intersite connectivity 


In this task, you will test connectivity between virtual machines on the three virtual networks that you connected via local and global peering in the previous task.


1. In the Azure portal, search for and select **Virtual machines**.

1. In the list of virtual machines, click **az104-05-vm0**.

1. On the **az104-05-vm0** blade, click **Connect**, in the drop-down menu, click **RDP**, on the **Connect with RDP** blade, click **Download RDP File** and follow the prompts to start the Remote Desktop session.

 > **Note**: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.
 
 > **Note**: You can ignore any warning prompts when connecting to the target virtual machines.


1. When prompted, sign in by using the **Student** username and **Pa55w.rd1234** password.

1. Within the Remote Desktop session to **az104-05-vm0**, right-click the **Start** button and, in the right-click menu, click **Windows PowerShell (Admin)**.

1. In the Windows PowerShell console window, run the following to test connectivity to **az104-05-vm1** (which has the private IP address of **10.51.0.4**) over TCP port 3389:

   ```pwsh
   Test-NetConnection -ComputerName 10.51.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

 > **Note**: The test uses TCP 3389 since this is this port is allowed by default by operating system firewall. 


1. Examine the output of the command and verify that the connection was successful.

1. In the Windows PowerShell console window, run the following to test connectivity to **az104-05-vm2** (which has the private IP address of **10.52.0.4**):

   ```pwsh
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

1. Switch back to the Azure portal on your lab computer and navigate back to the **Virtual machines** blade. 

1. In the list of virtual machines, click **az104-05-vm1**.

1. On the **az104-05-vm1** blade, click **Connect**, in the drop-down menu, click **RDP**, on the **Connect with RDP** blade, click **Download RDP File** and follow the prompts to start the Remote Desktop session.

 > **Note**: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.

 > **Note**: You can ignore any warning prompts when connecting to the target virtual machines.

1. When prompted, sign in by using the **Student** username and **Pa55w.rd1234** password.

1. Within the Remote Desktop session to **az104-05-vm1**, right-click the **Start** button and, in the right-click menu, click **Windows PowerShell (Admin)**.

1. In the Windows PowerShell console window, run the following to test connectivity to **az104-05-vm2** (which has the private IP address of **10.52.0.4**) over TCP port 3389:

   ```pwsh
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

 > **Note**: The test uses TCP 3389 since this is this port is allowed by default by operating system firewall. 

1. Examine the output of the command and verify that the connection was successful.

**Review**

In this lab, you have:

- Provisioned the lab environment
- Configured local and global virtual network peering
- Tested intersite connectivity
