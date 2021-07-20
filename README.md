# Introduction

Port Scanners are primarily used for Penetration Testing and Information Gathering. Essentially, we are looking for open ports in a host for one of two reasons. To ensure our servers are secure or to exploit those of someone else. An unnecessarily opened port means vulnerability and comes with a lack of security.
Therefore, it is reasonable to scan the ports of your own network in order to spot potential security gaps. To do so, we can use a popular and professional open-source software like Nmap.

# Basic Funciality
So a portscanner tries to connect  an IP-Address on a certain port. When we surf web we usually connects to a port to access the servers, eg. HTTP(port 80) ,FTP(port 21), SSH(port22), or port 443 (HTTPS). There are more than 130,000 ports of which 1,023 are standardized and 48,128 reserved. 

Now, we will first look at the simplest way to scan ports with Python.
```
def portscan(port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.connect((target, port))
        return True
    except:
        return False
```
We have created a ordinary socket that tries to connect to the ip-address and if it connects then give result true or other else give false.

we can also automate that by making a for-loop that scans all the standardized ports.

```
for port in range(1, 1024):
    result = portscan(port)
    if(result):
        print("Port {} is open!".format(port))
    else:
        print("Port {} is closed!".format(port))
```
  
  
# Initialize the ports
  * Libraries:
    First we need to initialize some libraries into the code 
```
import socket
```











