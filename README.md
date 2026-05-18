# Web Reconnaissance Detection Engineering Lab

## Objective

The purpose of this lab was to simulate early stage web reconnaissance activity in a controlled environment and identify observable indicators that could support SOC detection engineering use cases.

The focus was on identifying exposed services, performing basic web enumeration, and analysing HTTP responses that may reveal technology information prior to exploitation attempts.

---

## Lab Environment

| Component | Description |
|---|---|
| Platform | TryHackMe AttackBox |
| Target Environment | TryHackMe Lab Machine |
| Target IP | 10.130.74.163 |
| Tools Used | Nmap, Curl |
| Activity Type | Simulated Web Reconnaissance |

---

## Methodology

### Step 1: Verify Host Connectivity

Initial connectivity to the target machine was validated.

```bash
ping 10.130.74.163
```

Successful responses confirmed the target was online and reachable.

---

### Step 2: Perform Service Discovery

A fast TCP scan was performed to identify exposed services while disabling DNS lookups.

```bash
nmap -Pn -n -F 10.130.74.163
```

### Findings

```text
PORT     STATE SERVICE
22/tcp   open  ssh
53/tcp   open  domain
80/tcp   open  http
81/tcp   open  hosts2-ns
111/tcp  open  rpcbind
389/tcp  open  ldap
3389/tcp open  ms-wbt-server
6001/tcp open  X11:1
```

The scan identified multiple externally exposed services, including SSH, HTTP, LDAP and RDP.

![service scan](screenshots/nmspervicescan.png)

---

### Step 3: Enumerate Web Service Headers

HTTP header analysis was performed to identify server technologies and exposed metadata.

```bash
curl -I http://10.130.74.163
```

### Findings

```text
HTTP/1.1 405 Method Not Allowed
Server: WebSockify Python/3.8.10
Date: Mon, 18 May 2026
```

The response exposed backend technology information and revealed use of WebSockify with Python.

This type of information disclosure may assist adversaries during reconnaissance phases.

![headers](screenshots/headers.png)

---

### Step 4: Investigate Secondary Web Service

A secondary service on port 81 was manually investigated.

Browser access:

```text
http://10.130.74.163:81
```

Additional review:

```bash
curl http://10.130.74.163:81
```

### Findings

The service returned an Apache default page indicating a potentially exposed or default configuration.

Observed:

```text
Apache2 Ubuntu Default Page
"It works!"
```

![apache page](screenshots/apache.png)

---

## Detection Opportunities

Potential SOC detections:

- Alert on repeated requests across multiple ports
- Monitor abnormal web enumeration behaviour
- Detect repeated HTTP header probing activity
- Identify service fingerprinting attempts
- Correlate reconnaissance activity with source IP behaviour

---

## Key Lessons Learned

- Early reconnaissance creates observable indicators before exploitation begins
- HTTP responses can reveal technology information useful to attackers
- Multiple exposed services increase attack surface
- Web enumeration activity can provide opportunities for early detection

---

## Future Improvements

- Integrate traffic into a SIEM workflow
- Create custom detection rules
- Generate alerts for reconnaissance behaviour
- Expand into web attack simulation and log analysis
