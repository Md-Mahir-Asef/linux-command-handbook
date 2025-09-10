# Linux Handbook
This is a cheat sheet for Linux commands and Linux workflows.

## Common Concepts
### Networking
#### 1. Socket Vs Port:
- Port → A number that identifies a specific service or application on a machine (e.g., 80 for HTTP, 5432 for PostgreSQL).
- Socket → The actual communication endpoint, which combines:
  + IP address (which machine)
  + Port number (which service)
  + Protocol (TCP/UDP)
##### Analogy:
- Port = a room number in a building.
- Socket = the door to that room that’s actually open for someone to walk through and talk.

## Common Commands
### Networking
1. List all listening network sockets on your machine and which process owns them.
```bash
sudo netstat -tulpn
```
Example output:
```bash
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
Columns:
1. Proto → Protocol of the socket
