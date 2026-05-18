# Web Reconnaissance Detection Engineering Lab

## Objective

The purpose of this lab was to simulate early-stage web reconnaissance activity within a controlled environment and identify observable indicators relevant to SOC detection engineering.

The focus was on identifying exposed services, performing web enumeration and analysing HTTP responses that may reveal technology information prior to exploitation attempts.

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

Validated that the target machine was reachable.

```bash
ping 10.130.74.163
```

---

### Step 2: Perform Service Discovery

A fast TCP scan was performed to identify exposed services while disabling DNS lookups.

```bash
nmap -Pn -n -F 10.130.74.163
```

Findings:

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

---

## Evidence & Screenshots

### Service Discovery Scan

![service scan](nmap%20service%20scan.png)

Initial scanning identified multiple exposed services including SSH, HTTP, LDAP and RDP.

---

### HTTP Header Enumeration

Command used:

```bash
curl -I http://10.130.74.163
```

Observed:

```text
HTTP/1.1 405 Method Not Allowed
Server: WebSockify Python/3.8.10
```

![headers](curl%20page%20content.png)

HTTP response headers revealed backend technology information and identified a WebSockify Python service.

---

### Web Application Discovery

Browser investigation:

```text
http://10.130.74.163:81
```

Observed:

```text
Apache2 Ubuntu Default Page
"It works!"
```

![apache page](Apache%20page%20in%20browser.png)

Investigation of the secondary web service identified an exposed Apache default page.

---

## Detection Opportunities

- Alert on repeated requests across multiple ports
- Monitor abnormal web enumeration behaviour
- Detect HTTP header probing activity
- Identify service fingerprinting attempts
- Correlate reconnaissance behaviour with source activity

---

## Key Lessons Learned

- Early reconnaissance creates observable indicators before exploitation
- HTTP responses can reveal technology information useful to attackers
- Multiple exposed services increase attack surface
- Web enumeration activity creates opportunities for early detection

---

## Future Improvements

- Integrate activity into a SIEM workflow
- Create custom detection rules
- Generate alerts for reconnaissance behaviour
- Expand into web attack simulation and log analysis
