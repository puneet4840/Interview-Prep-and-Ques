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
