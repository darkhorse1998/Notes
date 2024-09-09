# DEBUGGING SLOW SERVER or WEBAPP FAILURE

> ISOLATE & NARROW DOWN the problem area

1. Slow server might not have any kinds of fruitful logs. It is not an error for the system. Although, it could a failure for the business as SLAs might get violated.
2. We can try a ping to the server, but most likely we will get a response, as the server has not stopped but just responding slow.
3. Check free memory by `free -m`. It gives information about total , used and available space of physical memory and swap memory. Swap memory, also known as swap space, is a section of a computer's hard disk or SSD that the operating system (OS) uses to store inactive data from Random Access Memory (RAM). This allows the OS to run even when RAM is full, preventing system slowdowns or crashes. But, swap memory can be slower than accessing data directly from physical memory.
4. Check the CPU% & Memory% by using `htop` or `top`. **htop** is interactive.
5. Check disk I/O performance using `iotop`
6. Check _/var/log/messages_ or _/var/log/syslog_ and application server for logs about STDERR or STDOUT
7. Examine incoming request and check if it can be isolated to a particular request URL with certain headers.
8. Any downstream APIs or DBs being called and responding slowly.
9. It could be a DDoS (Distributed Denial of Service) attack. Check if there a set of IPs continuously hammering the server.
10. If any reverse proxy is being used like nginx, try restarting them. Proxy or forward proxy sits in front of the client. A forward proxy can mask the client's IP address and location, and block access to specific websites. Reverse proxy sits ifront of the server. A reverse proxy forwards requests to other servers and returns the results to the client. A reverse proxy can act as a layer of abstraction between the client and the server. \
![image](https://github.com/user-attachments/assets/d9fc15e4-b2db-481d-a79e-f8de60d3c24a)

