# Networking Commands

<img src="https://drive.google.com/uc?export=view&id=1yMeNIhGNrghjareFsVPWZCUIQQULURWV" width="450" height="620">

<br>

### ping

Kya hota hai:
- ```ping``` ek network diagnostic tool hai jo host aur server ke beech mein network connectivity check karta hai, jaise ki ek machine (jaise ek server, ek website ya ek IP address) doosri machine se reachable hai ya nahi.
- ```ping``` = **Packet Internet Groper**.

<br>

**Ping kaise kaam karta hai?**:

Ping ICMP (Internet Control Message Protocol) ke upar kaam karta hai. Ye TCP/IP suite ka ek protocol hai jo mainly error reporting aur network diagnostics ke liye use hota hai.
- Jab tum ping ```www.google.com``` chalate ho:
  - System sabse pehle DNS lookup karta hai (agar domain diya hai) aur us domain ka IP address nikalta hai.
  - Fir wo ek ICMP Echo Request packet bhejta hai us IP address par.
- Agar destination system available hai aur firewall/blocking nahi hai:
  - Server ek ICMP Echo Reply packet wapas bhejega.
  - Jisse pta chalega kis server reachable hai.
 
- Tumhare terminal par dikhne wale output mein hota hai:
  - Packet size (bytes).
  - Destination IP.
  - Sequence number (jaise icmp_seq=1, icmp_seq=2 …).
  - TTL (Time to Live).
  - Round Trip Time (time=ms).
 
**Output samajhna**:
```
PING google.com (142.250.193.78) 56(84) bytes of data.
64 bytes from bom12s01-in-f14.1e100.net (142.250.193.78): icmp_seq=1 ttl=118 time=32.5 ms
64 bytes from bom12s01-in-f14.1e100.net (142.250.193.78): icmp_seq=2 ttl=118 time=31.8 ms
```
- 64 bytes → ICMP packet ka size.
- icmp_seq=1, 2… → Kaunsa packet hai sequence wise.
- ttl=118 → Maximum hops jo packet cross kar sakta hai (har router TTL 1 se decrement karta hai, agar 0 ho jaye to packet drop ho jata hai).
- time=32.5 ms → Packet ko jane aur wapas aane ka time (latency).

<br>

**Ping command options**:

1 - Specific packets bhejne ke liye:
- By default ```ping``` continuously chalta hai jab tak tum usko ```Ctrl+C``` se stop na karo.
- Agar suppose 4 packets behjne hain to hum ye ```-c``` option ka use karenge.
```
ping -c 4 www.google.com
```
Ye sirf 4 ICMP packets bhejega aur result dega.

2 - Specific time ke baad packets bhejne ke liye:
- By default, ```ping``` har 1 second ke time gap se packets bhejta hai.
- Agar ek particular time gap se packets bhejne hain to ```-i``` option ka use karenge.
```
ping -i 2 www.google.com
```
Ye har 2 seconds ke gap ke baad packets bhejega.

3 - Packet size (bytes) specify karne ke liye:
- By default ```ping``` 64 bytes ka packet bhejta hai.
- Agar alag size ka packet bhejna hai to ```-s``` option ka use karenge.
```
ping -s 100 www.google.com
```
Ye 100 bytes ka packets bhejega.

<br>
<br>

### ifconfig

Kya hota hai:
- ```ifconfig``` ka full form hai Interface Configuration.
- Ye ek utility hai jo Linux/Unix systems mein use hoti thi network interfaces (jaise Ethernet, WiFi, loopback, etc.) ko configure aur manage karne ke liye.
- Isse tum apne system ke network interfaces ka status, IP address, MAC address, aur configuration dekh sakte ho.

Note: Aajkal ke naye Linux distributions (Ubuntu 18.04+, CentOS 7+) mein ifconfig ko deprecated kar diya gaya hai, aur uski jagah ip command (ip addr, ip link) ka use hota hai. Lekin interview aur practical knowledge ke liye ifconfig zaroori hai.

Example: 
- Interfaces ke details dekhna:
```
ifconfig
```
Ye tumhare system ke sabhi network interfaces ka detail dikhata hai:
- Interface name (eth0, wlan0, lo, etc.).
- IP address (inet).
- Netmask.
- Broadcast address.
- MAC address (ether).
- RX/TX packets, errors, dropped packets.

<br>
<br>

### ip

Kya hota hai:
- ```ip``` ek command-line utility hai jo Linux mein networking interfaces, routing tables aur IP addresses ko manage karne ke liye use hoti hai.
- Ye iproute2 package ka hissa hai.
- Ye purane ifconfig, route aur netstat commands ka alternative hai.

Example:
- **ip address dekhna**:
```
ip show addr
```
Ye sabhi network interfaces ke IP addresses dikhata hai (IPv4 + IPv6).

- **Interfaces ka status dekhna**:
```
ip link show
```

<br>
<br>

### netstat

```netstat``` ka full form hai Network Statistics.

Ye command line utility hai jo tumhare system ke network ki information dikhata hai.

Matlab: Ye tumhe bata sakta hai ki kaunse ports open hain, kaunse connections active hain, kitna data bheja/receive hua hai, aur routing ka status kya hai.

**Note**: ```netstat``` ab deprecated hai aur naye Linux systems mein ```ss``` command use karna recommend kiya jata hai. Lekin abhi bhi ```netstat``` knowledge zaroori hai.

<be>

**netstat ke important options**:
- Active connections dekhna:
```
netstat
```
By default sabhi active connections, sockets, aur routing tables ka status dikhata hai.

- Listening ports dekhna:
```
netstat -l
```
Sirf woh ports dikhata hai jo listening state mein hain (yaani server kisi client ki request ka wait kar raha hai).

- Process ke sath ports dekhna:
```
netstat -lp
```

```-p``` option se pata chalega kaunsa process kaunsa port use kar raha hai.

Example output:
```
tcp   0   0 0.0.0.0:22   0.0.0.0:*   LISTEN   1234/sshd
```

Yaha ```1234/sshd``` ka matlab hai ki port 22 (SSH) process sshd use kar raha hai.

- TCP connections dekhna:
```
netstat -t
```
Sirf TCP protocol ke connections dikhata hai.

- UDP connections dekhna:
```
netstat -u
```

- Continuous output (monitoring ke liye):
```
netstat -c
```
Har second network stats update hote rahenge.

- Routing table dekhna:
```
netstat -r
```
Ye tumhare system ka routing table dikhata hai

- Numeric output:
```
netstat -n
```
Ye output mein sirf IP addresses dikhata hai, hostnames nahi.

- Sab combined ek jagah:
```
netstat -tulnp
```
- ```-t``` → TCP
- ```-u``` → UDP
- ```-l``` → Listening
- ```-n``` → Numeric (no DNS)
- ```-p``` → Process

Ye sabse common aur powerful command hai jo tumhe sabhi open ports aur unke processes dikhata hai.

Example Output Explanation:
```
netstat -tulnp
```
Output:
```
Proto Recv-Q Send-Q Local Address      Foreign Address     State       PID/Program name
tcp   0      0     0.0.0.0:22          0.0.0.0:*          LISTEN      1234/sshd
udp   0      0     127.0.0.1:323       0.0.0.0:*                     567/chronyd
```
Explanation:
- Proto → Protocol (TCP/UDP).
- Recv-Q → Receive queue (pending data from client).
- Send-Q → Send queue (pending data to send).
- Local Address → Local IP + port jo system use kar raha hai.
- Foreign Address → Remote IP + port jo connect hai.
- State → Connection ka state (LISTEN, ESTABLISHED, TIME_WAIT, etc.).
- PID/Program name → Process ID aur program jo port use kar raha hai

<br>
<br>

### nslookup

``nslookup``` ka full form hai Name Server Lookup.

Ye ek command-line tool hai jo DNS (Domain Name System) se related queries karne ke liye use hota hai.

Iske through tum check kar sakte ho:
- Kisi domain ka IP address kya hai.
- Kisi IP ka domain (reverse lookup).
- DNS record types (A, AAAA, MX, TXT, NS, CNAME, etc.).

Matlab: Agar tumhe doubt hai ki DNS sahi kaam kar raha hai ya nahi, sabse pehle ```nslookup``` use hota hai.


