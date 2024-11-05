# vpn
Free VPN for Servers

## Create .htpasswd 
1. Linux
Most Linux distributions come with Apache installed, which includes the htpasswd utility. If it's not installed, you can typically install it through your package manager.

For Ubuntu/Debian:

```
sudo apt update
sudo apt install apache2-utils
```
For CentOS/RHEL:

```
sudo yum install httpd-tools
```
2. macOS
On macOS, you can install htpasswd using Homebrew, a package manager for macOS:

```
brew install httpd
```
After installation, you can find the htpasswd command in /usr/local/bin/.

3. Windows
On Windows, the htpasswd utility is not natively included, but you can use it as part of an Apache installation or download a standalone version.

Using WAMP/XAMPP: If you have a WAMP or XAMPP server installed, the htpasswd tool is included with it.

Open the command prompt and navigate to the Apache bin directory (e.g., C:\wamp\bin\apache\apache2.x.x\bin or C:\xampp\apache\bin).
Run htpasswd from there.
Standalone Version: You can also download a precompiled binary for Windows from repositories like GnuWin32 or similar sites. Follow the installation instructions provided on the site.

4. Using htpasswd
Once you have htpasswd installed, you can create a password file using the following command:

Create or Update a Password File
To create a new .htpasswd file or update an existing one, use the following command:


```
htpasswd -c /path/to/.htpasswd username
```
The -c option creates the file. Omit it if you’re updating an existing file.
Replace /path/to/.htpasswd with the actual path where you want the .htpasswd file to be created.
Replace username with the desired username.
Example Command

```
htpasswd -c nginx/.htpasswd username
```

## VPN
Setting Up Wireguard and Wireguard UI with Docker Compose¶ Introduction to Wireguard and Wireguard UI¶ Wireguard is a modern VPN (Virtual Private Network) software that provides fast and secure connections. The Wireguard UI is a web interface that makes it easier to manage your Wireguard setup.

Docker Compose Configuration for Wireguard and Wireguard UI¶ This Docker Compose setup deploys both Wireguard and Wireguard UI in Docker containers, ensuring a secure, isolated environment for your VPN needs.

Docker Compose File (docker-compose.yml)¶ Issue with latest image

There is an issue with the latest image it seems, please make sure you use the image in the example compose below. If you use latest, the steps in this guide will not work.

Key Components of the Configuration¶ Wireguard Service¶ Image: linuxserver/wireguard:v1.0.20210914-ls7. 
Capabilities: NET_ADMIN for network management. Volumes: Maps ./config to /config in the container for configuration storage. 
Ports: Exposes port 5000 for web interface and 51820 for UDP traffic. 
Wireguard UI Service¶ Image: ngoduykhanh/wireguard-ui:latest. 
Dependence: Depends on the wireguard service. 
Capabilities: NET_ADMIN for network management. 
Network Mode: Uses the network of the wireguard service. 
Environment Variables: Configuration for email notifications, session management, and Wireguard UI settings. 
Logging: Configures log file size and format. 
Volumes: Maps ./db for database storage and ./config for Wireguard configuration. Deploying Wireguard and Wireguard UI¶ Save the above Docker Compose configuration in a docker-compose.yml file. Run docker-compose up -d to start the containers in detached mode. Access Wireguard UI via http://:5000 and configure your VPN. Configuring and Using Wireguard and Wireguard UI¶ After deployment, use the Wireguard UI to manage your Wireguard VPN settings, including adding and configuring VPN clients.

Post Up¶
```
iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE 
```
Post Down¶
```
iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
```

 The "Post Up" command and the "Post Down" command are used in the configuration of WireGuard to set up and tear down network routing rules for the WireGuard interface.

The "Post Up" command performs the following actions:

It adds a rule to the FORWARD chain of the iptables firewall to accept incoming traffic on the WireGuard interface (wg0). This allows packets to be forwarded between the WireGuard network and other networks. It adds a rule to the POSTROUTING chain of the iptables NAT (Network Address Translation) table to perform MASQUERADE on outgoing packets from the WireGuard interface (wg0) before they are sent out through the eth0 interface. MASQUERADE modifies the source IP address of the packets to match the IP address of the eth0 interface, allowing the response packets to be correctly routed back to the WireGuard network. The "Post Down" command reverses the actions performed by the "Post Up" command:

It deletes the rule from the FORWARD chain of the iptables firewall that accepts incoming traffic on the WireGuard interface (wg0). It deletes the rule from the POSTROUTING chain of the iptables NAT table that performs MASQUERADE on outgoing packets from the WireGuard interface (wg0). These commands are typically used when configuring a WireGuard VPN server in scenarios where Network Address Translation (NAT) is involved, such as when the server is behind a router performing NAT.