# Lab0


### Answers

####  Which website did you access?

- My website: [https://www.youtube.com](https://www.youtube.com/)

####  What are the IP address and port number of the website server?
<img width="1155" height="776" alt="2 1~2 2" src="https://github.com/user-attachments/assets/286c7c8f-39ea-49f4-8266-951c08904988" />

- Server IP address: `___142.250.192..134_______`
- Server port: `__48744________`
- Evidence: the destination IP and port in the captured packet


####  What is the process of the TCP three-way handshake?

- `SYN`:
- <img width="1142" height="792" alt="2 3SYN" src="https://github.com/user-attachments/assets/c104a386-d8a4-43cc-8394-6314336873ac" />

- `SYN-ACK`:
<img width="1156" height="805" alt="2 3 SYN-ACK" src="https://github.com/user-attachments/assets/d919fd05-8c22-42bb-9a56-f194a7f90e14" />

- `ACK`: `
- <img width="1145" height="767" alt="2 3ACK" src="https://github.com/user-attachments/assets/b947756e-1bac-4bff-9201-069dc3ea312c" />

---

## 2. DNS Packet Analysis


![Your screenshot: DNS packet list](images/dns-packet.png)

![Your screenshot: DNS packet details showing protocol layers](images/dns-layer.png)


#### 2.1 What are the IP address and port number of the DNS server?
<img width="1190" height="758" alt="image" src="https://github.com/user-attachments/assets/2e0c5189-2b24-4708-b82d-640169947985" />

- DNS server IP address: `__10.0.2.15________`
- DNS server port: `____53______`

#### 2.2 What is the domain name in the DNS query?

- Domain name: `____www.youtube.com______`

#### 2.3 Which protocol(s) does this DNS packet use?

Layer 2 (Link Layer): `___Ethernet II_______`
Layer 3 (Network Layer): `____Internet Protocol Version 4______`
Layer 4 (Transport Layer): `_____User Datagram Protocol_____`
Layer 5 (Application Layer): `____Domain Name System______`

---

## 3. Access an HTTP Page

### Answers

#### 3.1 Which HTTP page did you access?


- HTTP page: `____(http://www.gzxyzn.com/Article/bjrk2/1644.html)_____`

#### 3.2 What are the IP address and port number of the server hosting this page?
<img width="1106" height="722" alt="4 1~4 2" src="https://github.com/user-attachments/assets/593cbdfe-a043-4a7c-911f-bb17d02f58d5" />

- Server IP address: `__61.183.8.129_`
- Server port: `____80______`

#### 3.3 What is the HTTP request method?
<img width="1097" height="720" alt="4 3 get" src="https://github.com/user-attachments/assets/58c6523c-56ac-40b8-bbdb-63d95149376f" />

- Request method: `____GET______`

#### 3.4 What is the HTTP response status code, and what does it mean?
<img width="1090" height="767" alt="4 4" src="https://github.com/user-attachments/assets/f0a69079-82a8-4038-b291-bea11f1c984a" />

- Status code: `___200 OK_______`
- Meaning: `____The request was successful, and the server successfully returned the requested resource._____`

---

