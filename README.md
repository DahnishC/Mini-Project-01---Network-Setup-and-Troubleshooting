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
 ![2024-09-17 (8)](https://github.com/user-attachments/assets/a3f4b154-6ce2-4d0c-9769-28255d41f264)
   3. Restart the DNS service
      - Sudo systemct1 restart bind9
  4. Verify the DNS resolution’
      - From a workstation, run
        - Nslookup mywebserver.local

### Step 6: Test Connectivity
  - **Ping Test:**
      - From Workstation 1, ping the Web Server (ping 172.16.0.2) and DNS Server (ping 172.16.0.3).
  - **Access the Web Server:**
      - Open a browser on Workstation 1 and access http://172.16.0.2 to confirm web server functionality.
  - **DNS Test:**
      - Try resolving the domain using the DNS server by typing the domain name mywebserver.local into the browser on Workstation 1.
![2024-09-17 (7)](https://github.com/user-attachments/assets/a731dfc6-4cd1-4fdf-bfb1-23476f060ae5)


### Key Configuration Decisions and Justifications 
  - **Subnetting:**
      - **Workstations Subnet** (192.168.0.0/26): Allows up to 62 usable IP addresses for employee devices.
      - **Servers Subnet** (172.16.0.0/24): Allocates up to 254 usable IP addresses for future scalability in server deployment.
      - **IP Addressing:** Static IPs are assigned to all devices to ensure a reliable and predictable network setup, essential for server configurations.



### Troubleshooting Tips 
  - **Issue 1: No Connectivity Between LANs**
      - Solution: Ensure that IP routing is enabled on the router (ip routing command).
      - Verify that IP addresses are correctly assigned to the router interfaces.
  - **Issue 2: Unable to Access Web Server**
      - Solution: Check that the web server is up and running.
      - Ensure that there’s no firewall blocking port 80 (HTTP).
  - **Issue 3: DNS Resolution Failing**
      - Solution: Verify DNS settings on the server and ensure the DNS service is running.
      - Ensure the DNS record is properly configured.
   

## FAQ: Network Setup and Troubleshooting

### Q1: Why can’t Workstation 1 connect to the Web Server?
**Issue:** Workstation 1 (192.168.0.2) is unable to access the Web Server (172.16.0.2).
**Troubleshooting Steps:**
      1. **Check IP Configuration:** Ensure that the correct IP addresses and subnet masks are configured on both Workstation 1 and the Web Server.
        - Verify that Workstation 1 has 192.168.0.2/26 and the Web Server has 172.16.0.2/24.
      2. **Verify Default Gateway:** Ensure Workstation 1’s default gateway is correctly set to 192.168.0.1 and the Web Server’s default gateway is 172.16.0.1.
      3. **Test Connectivity:**
        - Ping the default gateway (192.168.0.1) from Workstation 1 to ensure it can reach the router.
        - Then, ping the Web Server (172.16.0.2) to confirm end-to-end communication.
**IMPORTANT:**
  - Always verify the IP configuration and the default gateway settings on devices when they cannot communicate across different subnets.


### Q2: Why are the workstations unable to communicate with each other on the same LAN?
**Issue:** Workstation 1 and Workstation 2 cannot communicate with each other even though they are on the same subnet.
**Troubleshooting Steps:**
  1. **Check Subnet Mask:** Ensure that both workstations are assigned the correct subnet mask. For the 192.168.0.0/26 network, the subnet mask should be 255.255.255.192.
  2. **Test Network Connectivity:** Ping Workstation 2 (192.168.0.3) from Workstation 1:
![2024-09-17 (10)](https://github.com/user-attachments/assets/57626c1a-5946-44e2-853d-4ba4c83fba40)

If there’s no reply, check the switch connections and verify that the switch ports are operational.
  3. **Switch Issues:** Ensure that both workstations are connected to Switch 1 and that the switch has no configuration issues (e.g., port shutdown or VLAN misconfiguration).
**IMPORTANT:**
  - Ensure that all devices on the same LAN have the same subnet mask and are physically connected to the same switch.

### Q3: Why can’t Workstation 1 access the Web Server despite being on the same network?
**Issue:** Workstation 1 is unable to access the Web Server (172.16.0.2), and the firewall on the Web Server (Windows PC) might be blocking traffic.
**Troubleshooting Steps:**
  1. **Check Windows Firewall Settings on the Web Server:**
      - Open Windows Defender Firewall on the Web Server.
      - Go to Advanced Settings to review the inbound rules
  2. **Temporarily Disable Windows Firewall for Testing:**
      - As a quick check, temporarily disable the Windows Firewall on the Web Server and test connectivity:
      - Go to Control Panel → Windows Defender Firewall.
      - Select Turn Windows Defender Firewall on or off.
      - Choose to disable it for Private and Public networks.
      - If disabling the firewall resolves the issue, the problem lies with firewall rules. Turn the firewall back on and review the rules carefully.
  3. **Check for Additional Security Software:**
      - If there is third-party security software installed (e.g., antivirus programs with a built-in firewall), verify that it is not blocking traffic to the Web Server. Temporarily disable any such software for testing purposes.
**IMPORTANT**
  - Ensure that firewall rules are specific and allow only necessary traffic to avoid opening potential vulnerabilities. Regularly audit firewall settings to maintain network security.

### Q4: Why can’t I access the DNS Server from Workstation 1?
**Issue:** Workstation 1 (192.168.0.2) is unable to reach the DNS Server (172.16.0.3), preventing domain name resolution.
**Troubleshooting Steps:**
  1. **Check DNS Server Configuration:**
      - Ensure that the DNS server service is running. For Windows Server, check via Services or the DNS Manager.
      - Ensure that the correct DNS record for the domain is configured (e.g., A Record pointing to a web server).
  2. **Verify IP Address and Default Gateway:**
      - Confirm that the DNS server has the correct IP configuration (172.16.0.3/24) and that its default gateway is set to the router’s interface IP (172.16.0.1).
  3. **Test DNS Resolution:**
      - On Workstation 1, test if you can resolve a domain name using the nslookup command:
      - If the DNS lookup fails, ensure that DNS traffic (port 53) is allowed through the firewall on the DNS Server.
  4. **Firewall Rules:**
      - On the DNS Server (Windows), check if there are firewall rules allowing inbound DNS traffic (UDP on port 53):
      - Go to Advanced Firewall Settings and ensure a rule for DNS traffic is enabled.
      - 
### Q5: How do I find my computer’s IP address
**Issue:** A user needs to know their IP address for troubleshooting.
**Steps to finding your IP Address:**
  1. **On Windows**
      - Click on the Start Menu
      - Type cmd in the search bar and open Command Prompt
      - Type ipconfig and press enter
      - Look for IPv4 Address in the results (e.g., 192.168.0.5).
  2. **On Mac**
      - Open Systems Prefrences and select Network
      - Click on your active network connection (Wi-Fi or Ethernet).
      - Your IP address will be displayed.

## Retrospective

### Challenges Encountered and How They Were Addressed
This hands-on experience setting up and troubleshooting a network taught me a lot about network configuration, problem-solving, and dealing with technical challenges that arise during a real-world network setup. There were multiple challenges that we as a group faced and one was when we were configuring the DNS Server, we encountered an issue where the domain name (mywebserver.local) was not resolving correctly to the Web erver’s IP address. Even after adding the A Record in the DNS configuration, requests for the domain name would fail. We realized that the DNS record was misconfigured because we had mistakenly entered the wrong IP address for the Web Server in the DNS settings. After correcting the A Record to point to the correct IP (172.16.0.2), the DNS Server successfully resolved the domain, and the Web Server became accessible by name. This challenge reinforced the importance of double-checking configuration settings, particularly with critical services like DNS. Another problem we faced was, Nahib’s laptop and Brady’s laptop were not being able to be pinged and were not able to reach the DNS server. We had to troubleshoot this problem and found out that on Windows, it is more strict with firewalls compared to mac’s. So on the DNS Server (Windows), we checked if there were firewall rules allowing inbound DNS traffic. For this we had to Go to Advanced Firewall Settings and ensure that the firewalls were turned off and finally got a connection!

### New Skills and Insights Gained
Throughout this project, I gained  a deeper understanding of how subnetting and IP addressing work in real-world network configurations. I had previously studied subnetting theoretically, but this project gave me a practical understanding of how to allocate IP addresses within subnets. The challenge of ensuring that all devices were properly configured and within the correct subnet range reinforced my understanding of how subnet masks function and how they affect the number of available IP addresses.  I also became more proficient in using Cisco Packet Tracer and the command-line interface (CLI) for configuring network devices. This hands-on experience with the CLI—configuring routers and switches, assigning IP addresses, enabling routing, and setting up services—was invaluable. I now feel more confident navigating the CLI and making configuration changes in a network environment.

### Areas for Improvement and What Could Be Done Diffrently 
The biggest room for improvement and for future projects in this class would be to improve on my notes during the project as doing this project I had to remember everything via my brain. While I kept notes of my steps, I did not document them as thoroughly as I should have, especially when it came to troubleshooting specific issues like DNS configuration and firewall rules. Better documentation of each step and configuration change would have made it easier to backtrack when I encountered problems. Moving forward, I plan to use network diagrams and configuration logs to ensure that every step is documented properly for easier troubleshooting and future reference.  




  



