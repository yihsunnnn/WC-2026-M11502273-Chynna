# Lab0: Basic Wireshark Operation and Capture

## Requirements

- Install Wireshark on your own computer.
- Capture packets for a public HTTPS website.
- Capture a DNS query packet.
- Access an unencrypted HTTP webpage and capture the HTTP request/response.
- Include your own screenshots as evidence.
- Answer the questions in each section.

---

## 1. Website Packet Capture

Choose and access any public HTTPS website other than the NTUST website.

### Questions

1. Which website did you access?
2. What are the IP address and port number of the website server?
3. What are the IP address and source port number of your PC when initially accessing the website?
4. What is the process of the TCP three-way handshake?
   - Identify the `SYN`, `SYN-ACK`, and `ACK` packets.
   - Briefly explain the purpose of each packet.

### Evidence

Insert your screenshot(s) here showing:
- the website opened in browser,
- the Wireshark capture,
- the `tcp` or `http` packet list,
- the `SYN`, `SYN-ACK`, and `ACK` packets.

![Your screenshot: Wireshark capture for the selected website](images/website-capture.png)

![Your screenshot: SYN / SYN-ACK / ACK handshake](images/tcp-handshake.png)

### Answers

#### 1.1 Which website did you access?

- My website: `__________`

#### 1.2 What are the IP address and port number of the website server?

- Server IP address: `__________`
- Server port: `__________`
- Evidence: the destination IP and port in the captured packet

#### 1.3 What are the IP address and source port number of your PC when initially accessing the website?

- PC IP address: `__________`
- Source port: `__________`
- Evidence: the source IP and source port in the captured packet

#### 1.4 What is the process of the TCP three-way handshake?

- `SYN`: `__________`
- `SYN-ACK`: `__________`
- `ACK`: `__________`

Explanation:
- `SYN`: the client sends a connection request to the server.
- `SYN-ACK`: the server acknowledges the request and agrees to establish the connection.
- `ACK`: the client confirms the connection, and the TCP connection is established.

---

## 2. DNS Packet Analysis

Use the following display filter:

```wireshark
dns
```

Find a DNS query packet related to your selected website.

### Questions

1. What are the IP address and port number of the DNS server?
2. What is the domain name in the DNS query?
3. Which protocols does this DNS packet use? List them from Layer 2 to Layer 5 of the TCP/IP five-layer model.

### Evidence

Insert your screenshot(s) here showing:
- the DNS packet list,
- the DNS query details,
- protocol layering from the packet details window.

![Your screenshot: DNS packet list](images/dns-packet.png)

![Your screenshot: DNS packet details showing protocol layers](images/dns-layer.png)

### Answers

#### 2.1 What are the IP address and port number of the DNS server?

- DNS server IP address: `__________`
- DNS server port: `__________`

#### 2.2 What is the domain name in the DNS query?

- Domain name: `__________`

#### 2.3 Which protocol(s) does this DNS packet use?

Layer 2 (Link Layer): `__________`
Layer 3 (Network Layer): `__________`
Layer 4 (Transport Layer): `__________`
Layer 5 (Application Layer): `__________`

---

## 3. Access an HTTP Page

Access a webpage that uses unencrypted HTTP instead of HTTPS.

Use the following display filter:

```wireshark
http
```

### Questions

1. Which HTTP page did you access?
2. What are the IP address and port number of the server hosting the page?
3. What is the HTTP request method?
4. What is the HTTP response status code, and what does it mean?

### Evidence

Insert your screenshot(s) here showing:
- the HTTP request,
- the HTTP response,
- the server IP and port,
- the HTTP status code.

![Your screenshot: HTTP request](images/http-request.png)

![Your screenshot: HTTP response and status code](images/http-response.png)

### Answers

#### 3.1 Which HTTP page did you access?

- HTTP page: `__________`

#### 3.2 What are the IP address and port number of the server hosting this page?

- Server IP address: `__________`
- Server port: `__________`

#### 3.3 What is the HTTP request method?

- Request method: `__________`

#### 3.4 What is the HTTP response status code, and what does it mean?

- Status code: `__________`
- Meaning: `__________`

---

## 4. Summary

In this lab, I used Wireshark to:
- capture packets from a public HTTPS website,
- analyze the DNS query packet,
- inspect an HTTP request and response.

This lab helped me understand how packets are transmitted and how Wireshark can be used to analyze the network layer, transport layer, and application layer behavior.

---

## 5. Link of the PCAP

- PCAP file link: `__________`

## 6. Video

- Video link: `__________`
