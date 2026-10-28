
## 📅 Week 1 – Work Completed

### 1. Installed VirtualBox
I installed the **latest recommended version of VirtualBox** on my host machine. It acts as the **base platform** for the whole lab.

### 2. Created the NAT Network
I created a NAT Network named **`LabNet`** with the subnet **`10.0.0.0/24`**. This gives the lab a **private, isolated network** that machines can be added to later.

### 3. Set Up the Kali Linux VM
I imported **Kali Linux** and configured it as the **attacking (hacker) machine**. I assigned suitable **RAM, CPU and disk resources** for running security tools.

### 4. Connected Kali to the NAT Network
I attached the Kali VM's network adapter to **`LabNet`**. This places Kali inside the **`10.0.0.0/24`** subnet while still allowing **Internet access**.

### 5. Enabled Clipboard and Drag & Drop
I set **Shared Clipboard** and **Drag'n'Drop** to **Bidirectional** in the VM settings. This makes it easy to **copy text and move files** between the host and Kali.

### 6. Configured the Shared Folder
I shared the host's **Downloads folder** with Kali using the name **`downloads`**, with **Auto-mount** and **Make Permanent** enabled. This lets me **exchange tools and files** with the host machine directly.

### 7. Assigned a Static IP to Kali
I configured Kali with the **static IP address `10.0.0.2/24`**. A fixed address keeps the lab **predictable** for scanning and testing later.

### 8. Verified the Setup
I tested the setup with **`ip a`** and **`ping`** commands. This confirmed the **correct IP**, **connectivity to the network** and **full Internet access** (including DNS).

