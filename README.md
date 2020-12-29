# Festive Calendar 2020 - Hands on lab

## Lab - Implement Intersite Connectivity

**Lab scenario**

Contoso has its datacenters in Boston, New York, and Seattle offices connected via a mesh wide-area network links, with full connectivity between them. You need to implement a lab environment that will reflect the topology of the Contoso's on-premises networks and verify its functionality. 

**Objectives**

In this lab, you will:

+ Task 1: Provision the lab environment
+ Task 2: Configure local and global virtual network peering
+ Task 3: Test intersite connectivity 

### Task 1: Provision the lab environment

1. Click the **Deploy Azure** button below:

	 [![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fgarethjones294%2Ffestivecal2020%2Fmain%2Fazuredeploy.json)

1. Login to the Azure Portal with your credentials.

1. XXXXXXXXXXXXXXX

	> **Note**: Wait for the deployments to complete before proceeding tothe next task. This should take about 5-15 minutes. To verify the status of the deployments, you can examine the properties of the resource groups you created in this task.

### Task 2: Configure local and global virtual network peering

In this task, you will configure local and global peering between the virtual networks you deployed in the previous tasks.

1. In the Azure portal, search for and select **Virtual networks**.

1. Review the virtual networks you created in the previous task and verify that the first two are located in the same Azure region and the third one in a different Azure region. 

	> **Note**: The template used for deployment creates three virtual networks and ensures that the IP address ranges of the three virtual networks do not overlap.


1. In the list of virtual networks, click **vnet0**.

1. On the **vnet0** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Add a peering with the following settings (leave others with their default values):

    | Setting | Value|
    | --- | --- |
    | Name of the peering from vnet0 to remote virtual network | **vnet0_to_vnet1** |
    | Virtual network deployment model | **Resource manager** |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **vnet1 (rg1)** |
    | Name of the peering from vnet1 to vnet0 | **vnet1_to_vnet0** |
    | Allow virtual network access from vnet0 to vnet1 | **Enabled** |
    | Allow virtual network access from vnet1 to vnet0 | **Enabled** |
    | Allow forwarded traffic from vnet1 to vnet0 | **Disabled** |
    | Allow forwarded traffic from vnet0 to vnet1 | **Disabled** |

	> **Note**: This step establishes two local peerings - one from vnet0 to vnet1 and the other from vnet1 to vnet0.


1. On the **vnet0** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Add a peering with the following settings (leave others with their default values):

    | Setting | Value|
    | --- | --- |
    | Name of the peering from vnet0 to remote virtual network | **vnet0_to_vnet2** |
    | Virtual network deployment model | **Resource manager** |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **vnet2 (rg2)** |
    | Name of the peering from vnet2 to vnet0 | **vnet2_to_vnet0** |
    | Allow virtual network access from vnet0 to vnet2 | **Enabled** |
    | Allow virtual network access from vnet2 to vnet0 | **Enabled** |
    | Allow forwarded traffic from vnet2 to vnet0 | **Disabled** |
    | Allow forwarded traffic from vnet0 to vnet2 | **Disabled** |

	> **Note**: This step establishes two global peerings - one from vnet0 to vnet2 and the other from vnet2 to vnet0.

 
9. Navigate back to the **Virtual networks** blade and, in the list of virtual networks, click **vnet1**.

1. On the **vnet1** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Add a peering with the following settings (leave others with their default values):

    | Setting | Value|
    | --- | --- |
    | Name of the peering from vnet1 to remote virtual network | **vnet1_to_vnet2** |
    | Virtual network deployment model | **Resource manager** |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Virtual network | **vnet2 (rg2)** |
    | Name of the peering from vnet2 to vnet1 | **vnet2_to_vnet1** |
    | Allow virtual network access from vnet1 to vnet2 | **Enabled** |
    | Allow virtual network access from vnet2 to vnet1 | **Enabled** |
    | Allow forwarded traffic from vnet2 to vnet1 | **Disabled** |
    | Allow forwarded traffic from vnet1 to vnet2 | **Disabled** |

 	> **Note**: This step establishes two global peerings - one from vnet1 to vnet2 and the other from vnet2 to vnet1.


### Task 3: Test intersite connectivity 


In this task, you will test connectivity between virtual machines on the three virtual networks that you connected via local and global peering in the previous task.


1. In the Azure portal, search for and select **Virtual machines**.

1. In the list of virtual machines, click **vm0**.

1. On the **vm0** blade, click **Connect**, in the drop-down menu, click **RDP**, on the **Connect with RDP** blade, click **Download RDP File** and follow the prompts to start the Remote Desktop session.

 	> **Note**: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.
 
 	> **Note**: You can ignore any warning prompts when connecting to the target virtual machines.


1. When prompted, sign in by using the **Student** username and **Pa55w.rd1234** password.

1. Within the Remote Desktop session to **vm0**, right-click the **Start** button and, in the right-click menu, click **Windows PowerShell (Admin)**.

1. In the Windows PowerShell console window, run the following to test connectivity to **vm1** (which has the private IP address of **10.51.0.4**) over TCP port 3389:

   ```pwsh
   Test-NetConnection -ComputerName 10.51.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

 	> **Note**: The test uses TCP 3389 since this is this port is allowed by default by operating system firewall. 


1. Examine the output of the command and verify that the connection was successful.

1. In the Windows PowerShell console window, run the following to test connectivity to **vm2** (which has the private IP address of **10.52.0.4**):

   ```pwsh
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

1. Switch back to the Azure portal on your lab computer and navigate back to the **Virtual machines** blade. 

1. In the list of virtual machines, click **vm1**.

1. On the **vm1** blade, click **Connect**, in the drop-down menu, click **RDP**, on the **Connect with RDP** blade, click **Download RDP File** and follow the prompts to start the Remote Desktop session.

 	> **Note**: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store and on Linux computers you can use an open source RDP client software.

 	> **Note**: You can ignore any warning prompts when connecting to the target virtual machines.

1. When prompted, sign in by using the **Student** username and **Pa55w.rd1234** password.

1. Within the Remote Desktop session to **vm1**, right-click the **Start** button and, in the right-click menu, click **Windows PowerShell (Admin)**.

1. In the Windows PowerShell console window, run the following to test connectivity to **vm2** (which has the private IP address of **10.52.0.4**) over TCP port 3389:

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
