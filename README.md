# Linux Handbook
This is a cheat sheet for Linux commands and Linux workflows.

## Common Concepts
### Networking
#### 1. Socket Vs Port:
**Socket (Formal Definition)**: A socket is one endpoint of a two-way communication link between two programs running on the network.<br>
**Port (Formal Definition)**: A port is a virtual point where network connections start and end.

Simply,
- Port → A number that identifies a specific service or application on a machine (e.g., 80 for HTTP, 5432 for PostgreSQL).
- Socket → The actual communication endpoint, which combines:
  + IP address (which machine)
  + Port number (which service)
  + Protocol (TCP/UDP)

##### Analogy:
- Port = a room number in a building.
- Socket = the door to that room that’s actually open for someone to walk through and talk.
  
#### 2. TCP6 & UDP6
TCP6 and UDP6 are not new protocols. They are the same TCP and UDP protocols running over IPv6 instead of IPv4.<br>
**They only say that the socket is using IPv6.**

#### 3. Network Interface
**Network Interface (Formal Definition)**:  A network interface is the defined boundary, both physical and logical, through which a host system exchanges data with a network.

A network interface is basically a “connection point” between your computer and a network.
- It can be physical (like an Ethernet port or Wi-Fi card) or virtual (like loopback lo, Docker bridge, or VPN adapters).
- Each interface has an IP address, which identifies it on that network.
- Examples on Linux:
  - lo → Loopback (127.0.0.1), for internal communication.
  - eth0 → Physical Ethernet card.
  - wlan0 → Wi-Fi adapter.
  - docker0 → Docker’s virtual network.

##### Analogy:
- Think of your computer as a house.
- Network interfaces are the doors/windows that let it connect to the outside world.
- Each door (interface) can have its own address, so people know where to send data.
💡 So when a socket binds to ```0.0.0.0```, it’s like saying: *“Listen on all doors/interfaces”*.

## Common Commands
### Networking
#### 1. List all listening network sockets on your machine and which process owns them.
```bash
sudo netstat -tulpn
```
Example output:
```
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      128/systemd-resolve
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      128/systemd-resolve
tcp        0      0 10.255.255.254:53       0.0.0.0:*               LISTEN      -
tcp6       0      0 :::15432                :::*                    LISTEN      -
udp        0      0 0.0.0.0:5353            0.0.0.0:*                           210/avahi-daemon: r
udp        0      0 0.0.0.0:40872           0.0.0.0:*                           210/avahi-daemon: r
udp        0      0 127.0.0.54:53           0.0.0.0:*                           128/systemd-resolve
udp        0      0 127.0.0.53:53           0.0.0.0:*                           128/systemd-resolve
udp        0      0 10.255.255.254:53       0.0.0.0:*                           -
udp        0      0 127.0.0.1:323           0.0.0.0:*                           -
udp6       0      0 :::5353                 :::*                                210/avahi-daemon: r
udp6       0      0 :::47337                :::*                                210/avahi-daemon: r
udp6       0      0 ::1:323                 :::*                                -
```
##### Columns:
1. **Proto** → Protocol of the socket. (Eg. tcp, udp, tcp6, udp6)
2. **Recv-Q** (Receive Queue) → Number of bytes received by the kernel but not yet read by the application. Usually ```0``` if the application is keeping up. High numbers can indicate the process isn’t reading data fast enough.
3. **Send-Q** (Send Queue) → Number of bytes sent by the kernel but not yet acknowledged by the remote side. Usually ```0``` for LISTEN sockets. Large numbers could indicate network congestion or a stalled connection.
4. **Local Address** → The IP and port on your machine that the process is listening on.
Examples:
- ```127.0.0.1:323``` → localhost only, port 323.
- ```0.0.0.0:5353``` → all IPv4 interfaces, port 5353.
- ```:::15432``` → all IPv6 interfaces, port 15432.
5. **Foreign Address** → The IP and port of the remote endpoint. For LISTEN sockets, it’s usually 0.0.0.0:* or :::*, because it’s waiting for any connection. For active connections (ESTABLISHED), it would show the client IP:port.
6. **State** → Current state of the socket.
Common ones:
- ```LISTEN``` → Waiting for incoming connections.
- ```ESTABLISHED``` → Active connection exists.
- ```CLOSE_WAIT```, ```SYN_SENT```, etc. → Various TCP states.
Only TCP has states; UDP usually shows blank because it’s connectionless.
7. **PID/Program name** → The PID(Process ID) and program owning the socket. Format: PID/ProgramName.
Example:
- ```128/systemd-resolve``` → PID ```128``` is ```systemd-resolve```.
- ```-``` → PID unknown, often because it’s kernel-level or restricted namespace.
