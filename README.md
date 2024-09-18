# Mini-Project-01---Network-Setup-and-Troubleshooting
## Step-by-Step Configuration Guide

### Objective 
The objective is to configure a network with two Local Area Networks (LANs):
  - One LAN for workstations using the IP range 192.168.0.0/26.
  - One LAN for servers (Web and DNS) using the IP range 172.16.0.0/24. The router will connect both LANs and provide routing between them.

### Use Cases for the System 

**Internal Corporate Network:** Employees use workstations to access internal services like the web server.
**Server Maintenance and Hosting:** Network admins can access and maintain the web and DNS servers, ensuring external services run smoothly.
**Testing and Development:** Developers can test internal applications hosted on the web server in a secure environment.


### Detailed Configuration Steps

**Step (optional) Subnet Mask and IP Assignment Table**
  - Creating a table to outline the IP addressing plan for all devices can be helpful
![2024-09-17 (9)](https://github.com/user-attachments/assets/2f962210-9e99-4188-809c-c85b92e1dd8a)


**Step 1: Setting Up the Router**
  1. Assign IP Addresses to Router Interfaces:
    - Open the router’s CLI or web interface (if available).
     - Assign the following IP addresses:
          - GigabitEthernet0/0 (connected to LAN 1, the Workstations): 192.168.0.1/26
          - GigabitEthernet0/1 (connected to LAN 2, the Servers): 172.16.0.1/24
  2. Enable Routing (if needed)
    - If needed, enable IP routing to allow communication between LANs
    - Command
          - Router(config)# ip routing

### Step 2: Configuring the Switches
  1. **Switch 1 **(for the Workstations):
    - Connect Workstation 1 and Workstation 2 to Switch 1.
    - Ensure that Switch 1 is connected to GigabitEthernet0/0 on the router.
    - No specific IP configuration is needed for the switch unless it’s a managed switch.
    - **Connection Setup:**
         - Use copper straight-through cables to connect the devices.
  2. **Switch 2** (for the Servers):
    - Connect the Web Server and DNS Server to Switch 2.
    - Ensure that Switch 2 is connected to GigabitEthernet0/1 on the router.
    - **Connection Setup:**
        - Use copper straight-through cables for connections.

### Step 3: Configuring the Workstations
  1. **Workstation 1 Configuration:**
    - IP address: 192.168.0.2/26
    - Default gateway: 192.168.0.1
    - **Steps:**
        - Go to the network settings on the workstation and assign a static IP.
        - Enter 192.168.0.2 as the IP address, 255.255.255.192 as the subnet mask, and 192.168.0.1 as the default gateway.
    - **Verify with a Ping:**
        - Ping the router: ping 192.168.0.1
  2. **Workstation 2 Configuration:**
    - IP address: 192.168.0.3/26
    - Default gateway: 192.168.0.1
    - Follow the same steps as for Workstation 1.

### Step 4: Configuring the Servers
  1. **Web Server Configuration:**
    - IP address: 172.16.0.2/24
    - Default gateway: 172.16.0.1
    - **Steps:**
        - Access the web server’s network settings.
        - Assign the static IP 172.16.0.2, subnet mask 255.255.255.0, and default gateway 172.16.0.1.
  2. **DNS Server Configuration:**
    - IP address: 172.16.0.3/24
    - Default gateway: 172.16.0.1
    - Follow the same steps as the web server.

### Step 5: Configuring DNS on the DNS Server
  1. Install the DNS server software (if not installed).
  2. Add a DNS record for the web server:
      - **A Record:** Point the domain name (e.g., mywebserver.local) to the web server’s IP address (172.16.0.2).
        - Mywebserver.local.      IN A    172.16.0.2
  

Restart the DNS service
Sudo systemct1 restart bind9
Verify the DNS resolution’
From a workstation, run
Nslookup mywebserver.local

Step 6: Test Connectivity
Ping Test:
From Workstation 1, ping the Web Server (ping 172.16.0.2) and DNS Server (ping 172.16.0.3).
Access the Web Server:
Open a browser on Workstation 1 and access http://172.16.0.2 to confirm web server functionality.
DNS Test:
Try resolving the domain using the DNS server by typing the domain name mywebserver.local into the browser on Workstation 1.


Key Configuration Decisions and Justifications 
Subnetting:
Workstations Subnet (192.168.0.0/26): Allows up to 62 usable IP addresses for employee devices.
Servers Subnet (172.16.0.0/24): Allocates up to 254 usable IP addresses for future scalability in server deployment.
IP Addressing: Static IPs are assigned to all devices to ensure a reliable and predictable network setup, essential for server configurations.



Troubleshooting Tips 
Issue 1: No Connectivity Between LANs
Solution: Ensure that IP routing is enabled on the router (ip routing command).
Verify that IP addresses are correctly assigned to the router interfaces.
Issue 2: Unable to Access Web Server
Solution: Check that the web server is up and running.
Ensure that there’s no firewall blocking port 80 (HTTP).
Issue 3: DNS Resolution Failing
Solution: Verify DNS settings on the server and ensure the DNS service is running.
Ensure the DNS record is properly configured.
