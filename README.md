# Web Reconnaissance Detection Engineering Lab

## Objective

The purpose of this lab was to simulate web reconnaissance activity and identify observable indicators relevant to SOC detection engineering.

---

## Tools Used

- TryHackMe AttackBox
- Nmap
- Curl

---

## Service Discovery

Command used:

```bash
nmap -Pn -n -F 10.130.74.163
```

Findings:

```text
22/tcp   open ssh
53/tcp   open domain
80/tcp   open http
81/tcp   open hosts2-ns
111/tcp  open rpcbind
389/tcp  open ldap
3389/tcp open ms-wbt-server
6001/tcp open X11:1
```

Screenshot:

![service scan](nmap%20service%20scan.png)

---

## HTTP Header Enumeration

Command used:

```bash
curl -I http://10.130.74.163
```

Findings:

```text
HTTP/1.1 405 Method Not Allowed
Server: WebSockify Python/3.8.10
```

Screenshot:

![headers](curl%20page%20content.png)

---

## Web Application Discovery

Browser investigation:

```text
http://10.130.74.163:81
```

Observed:

```text
Apache2 Ubuntu Default Page
"It works!"
```

Screenshot:

![apache page](Apache%20page%20in%20browser.png)

---

## Detection Opportunities

- Monitor repeated requests across multiple ports
- Detect web enumeration activity
- Monitor HTTP fingerprinting behaviour
- Identify reconnaissance patterns

---

## Lessons Learned

- Web reconnaissance generates observable activity
- HTTP responses may reveal technology information
- Service exposure increases attack surface
- Early detection opportunities exist before exploitation
