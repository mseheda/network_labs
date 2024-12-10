
# Snort Rule Report for Koi Stealer Detection

## Overview
This report details the Snort rules crafted to detect malicious activities associated with Koi Stealer malware. These rules are based on the identified Indicators of Compromise (IOCs) and traffic behaviors linked to Koi Stealer C2 communication and other suspicious activities.

---

## Rules

### 1. **Koi Stealer C2 Communication - POST /foots.php**
```snort
alert tcp any any -> 79.124.78.197 80 (msg:"Koi Stealer C2 traffic detected - POST /foots.php"; flow:to_server,established; content:"POST"; http_method; content:"/foots.php"; http_uri; sid:200001; rev:1;)
```
- **Purpose**: Detects HTTP POST requests to the Koi Stealer C2 server for the `/foots.php` endpoint.
- **Trigger Condition**:
  - Any POST request to IP `79.124.78.197` on port 80 with the URI `/foots.php`.
- **Explanation**:
  - This endpoint is part of the C2 communication for Koi Stealer. Detecting such requests helps identify infected devices communicating with the attacker-controlled server.

---

### 2. **Koi Stealer C2 Communication - POST /index.php**
```snort
alert tcp any any -> 79.124.78.197 80 (msg:"Koi Stealer C2 traffic detected - POST /index.php"; flow:to_server,established; content:"POST"; http_method; content:"/index.php"; http_uri; sid:200002; rev:1;)
```
- **Purpose**: Detects HTTP POST requests to the C2 server at the `/index.php` endpoint.
- **Trigger Condition**:
  - Any POST request to IP `79.124.78.197` on port 80 with the URI `/index.php`.
- **Explanation**:
  - This rule detects an alternative endpoint for Koi Stealer C2 communication, ensuring broader coverage of malicious activity.

---

### 3. **Koi Stealer C2 Communication - POST with Query Parameters**
```snort
alert tcp any any -> 79.124.78.197 80 (msg:"Koi Stealer C2 traffic detected - POST with query params"; flow:to_server,established; content:"POST"; http_method; content:"index.php?id&subid=qIOuKk7U"; http_uri; sid:200003; rev:1;)
```
- **Purpose**: Identifies POST requests to the `/index.php` endpoint with specific query parameters.
- **Trigger Condition**:
  - Any POST request to IP `79.124.78.197` on port 80 with the URI `index.php?id&subid=qIOuKk7U`.
- **Explanation**:
  - Query parameters are often used to identify infected hosts or provide session details. This rule ensures detection of such specific behavior.

---

### 4. **Suspicious HTTPS Traffic to Compromised Domain**
```snort
alert tcp any any -> any 443 (msg:"Suspicious HTTPS traffic to bellantonicioccolato.it"; flow:to_server,established; content:"Host|3A| www.bellantonicioccolato.it"; http_header; sid:200004; rev:1;)
```
- **Purpose**: Detects HTTPS traffic to the potentially compromised domain `www.bellantonicioccolato.it`.
- **Trigger Condition**:
  - Any HTTPS traffic containing the `Host` header `www.bellantonicioccolato.it`.
- **Explanation**:
  - This domain has been identified as potentially compromised or associated with malicious activity. Monitoring such traffic helps detect and block communication with suspicious servers.

---

### 5. **Suspicious Activity from Infected Host**
```snort
alert ip 172.17.0.99 any -> any any (msg:"Suspicious activity from infected host (172.17.0.99)"; sid:200005; rev:1;)
```
- **Purpose**: Monitors traffic from a known infected host.
- **Trigger Condition**:
  - Any IP traffic originating from the host `172.17.0.99`.
- **Explanation**:
  - This rule is essential for tracking all activities from a compromised internal host, helping detect lateral movement or outbound malicious traffic.

---

## Why These Rules Are Usable

1. **Specific Targeting**:
   - Each rule is tailored to specific behaviors or indicators related to Koi Stealer malware and other suspicious activities.

2. **Comprehensive Coverage**:
   - Covers both C2 communication (HTTP POST requests) and broader suspicious activity, such as traffic to malicious domains and infected hosts.

3. **Behavior-Based Detection**:
   - Rules focus on behavioral patterns, such as specific HTTP requests or domain communications, making them resilient to minor changes in infrastructure by attackers.

---

## When Each Rule Triggers

1. **Koi Stealer C2 Communication - POST /foots.php**:
   - Triggered by a POST request to the `/foots.php` endpoint on the C2 server.

2. **Koi Stealer C2 Communication - POST /index.php**:
   - Triggered by a POST request to the `/index.php` endpoint on the C2 server.

3. **Koi Stealer C2 Communication - POST with Query Parameters**:
   - Triggered by a POST request to `/index.php` with the specific query parameters.

4. **Suspicious HTTPS Traffic to Compromised Domain**:
   - Triggered by any HTTPS traffic containing the `Host` header `www.bellantonicioccolato.it`.

5. **Suspicious Activity from Infected Host**:
   - Triggered by any IP traffic originating from the compromised host `172.17.0.99`.

---

## Summary

These rules provide robust detection capabilities for Koi Stealer malware and related suspicious activities. By monitoring C2 communication, malicious domains, and infected hosts, they offer comprehensive coverage for identifying and mitigating threats.

