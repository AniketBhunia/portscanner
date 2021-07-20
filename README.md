<h1><centre>PORT SCANNER USING PYTHON</centre></h1>
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
    <p>First we need to initialize some libraries into the code</p> 
```
import socket
```
As a single thred pogram the time it will take will be slow so we are going to multi-thred it.
```
import threading
from queue import Queue
```
* Socket will be used for our connection attempts to the host at a specific port.
* Threading will allow us to run multiple scanning functions simultaneously.
* Queue is a data structure that will help us to manage the access of multiple threads on a single resource, which in our case will be the port numbers. 

Then we are going to call three global variable that we will need throught the functions.

```
target = "Your IP address"
queue = Queue()
open_ports = []
```
* Target is obviously the IP-Address or domain of the host we are trying to scan.
* The queue is now empty and will later be filled with the ports we want to scan.
* And last but not least we have an empty list, which will store the open port numbers at the end.

Now implementing the portscanfunction
```
def portscan(port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.connect((target, port))
        return True
    except:
        return False
```
Before we going into the threading, we are going to define the ports we want to scan. For this, we will define another function called get_ports.
```
def get_ports(mode):
    if mode == 1:
        for port in range(1, 1024):
            queue.put(port)
    elif mode == 2:
        for port in range(1, 49152):
            queue.put(port)
    elif mode == 3:
        ports = [20, 21, 22, 23, 25, 53, 80, 110, 443]
        for port in ports:
            queue.put(port)
    elif mode == 4:
        ports = input("Enter your ports (seperate by blank):")
        ports = ports.split()
        ports = list(map(int, ports))
        for port in ports:
            queue.put(port)
```

In this function we have defined four possible modes. The first mode scans the 1023 standardized ports. With the second mode we add the 48,128 reserved ports. By using the third mode we focus on some of the most important ports only. And finally, the fourth mode gives us the possibility to choose our ports manually. After that we add all our ports to the queue.

# Multithreading
The next thing we need to do is defining a so-called worker function for our threads. This function will be responsible for getting the port numbers from the queue, scanning them and printing the results.

```
def worker():
    while not queue.empty():
        port = queue.get()
        if portscan(port):
            print("Port {} is open!".format(port))
            open_ports.append(port)
```

So, now that we have implemented the functionality, we are going to write our main function, which creates, starts and manages our threads.

```
def run_scanner(threads, mode):

    get_ports(mode)

    thread_list = []

    for t in range(threads):
        thread = threading.Thread(target=worker)
        thread_list.append(thread)

    for thread in thread_list:
        thread.start()

    for thread in thread_list:
        thread.join()

    print("Open ports are:", open_ports)
```

In this function, we have two parameters. The first one is for the amount of threads we want to start and the second one is our mode. We load our ports, depending on the mode we have chosen and we create a new empty list for our threads. Then, we create the desired amount of threads, assign them our worker function and add them to the list.

```
run_scanner(100, 1)
```

Run it and provide your ip address that you want to scan and the output will be like this->
```
Port 135 is open!
Port 445 is open!
Port 902 is open!
Port 912 is open!
Open ports are: [135, 445, 902, 912]
```






