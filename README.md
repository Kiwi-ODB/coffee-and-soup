# coffee-and-soup
coffee store and soup kitchen need to communicate

# Rough plan

<img width="1390" height="750" alt="Coffee and Soup Plan" src="https://github.com/user-attachments/assets/a5b4f23c-d1d9-403c-94d9-300815b56dfb" />

# Doing it

## Coffee Network
### Coffee Router 
#### LAN
10.10.1.1
DHCP Lower - Upper: 10.10.1.100 - 10.10.1.200

#### WAN
192.168.0.20
Upstream:
192.168.0.1

### Coffee Server
IP static at 10.10.1.10

## Soup Network
### Soup Router

### LAN
10.10.0.1
DHCP Lower - Upper: 10.10.1.100 - 10.10.0.200

### WAN
192.168.0.60

### Soup Client
IP assigned by DHCP
In this instance: 10.10.0.139

<img width="1042" height="581" alt="image" src="https://github.com/user-attachments/assets/6734e1fe-d31a-4910-bc63-deef9dc3149e" />


## Issues I faced
<ul>
  <li>DNS Issue not resolving</li>
  The First issue I had with this network was that DNS was not resolving and instead resulted in "ServFail" responses I was unsure why this was happening

  ### Symptoms
  Ping 8.8.8.8 was working, ping google.com did not work
  Servfail would result after 1-3 MS which is not enough time to do a real look up
  Ping other router ip was working

  ### What fixed it
  As I am using OPNSense as a router it auto came with unbound, I looked into this further and because of my weird network set up having a real router upstream I needed to enable DNS query Forwarding, to use my main (real) router's DNS instead of my virtual router's 
  
  <li>Self Inflicted Primary console issue</li>
  While I was fixing the DNS issue I had to enter a long terminal command so I thought I should make it paste-able in the terminal rather than hand writing each command 
  
  ### Symptoms
  I decided I had to use the qm terminal command on my cache laptop so I could copy and paste into and from terminal
  To do this I had to change the primary console to serial port, This was a mistake as now I ONLY had access to it from the cache terminal.

  ### Fix
  I have undone this and decided to instead use a VM on the network to do any SSH requirements (by typing the commands as before) or WEB UI Access 
  
  <li>Incorrect Routing</li>

  ### Diagnosing
  When pinging from soup wifi to coffee webserver I was getting no response from pings, I have checked the coffee router logs and saw it was received and then passed, but when I checked the soup router there were no responses coming back:

  ### Fix
  My firewall rule was input incorrectly and I was passing web from the soup network back to the coffee network at 192.168.0.20 rather than the soup at 192.168.0.60


## working 
 Client on soup network was able to curl web server on coffee network

<img width="916" height="536" alt="image" src="https://github.com/user-attachments/assets/ddd96423-719e-4341-b5f7-60618086135f" />


## Questions

What is best practice to access different machines in homelab and professional way?
