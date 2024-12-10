
# Snort Rule Report for WarmCookie Detection

## Overview
This report details the Snort rules created to detect malicious activities associated with WarmCookie malware. These rules focus on identifying malicious file downloads, suspicious communications, and activities from compromised hosts.

---

## Rules

### 1. **WarmCookie ZIP Download Detected**
```snort
alert tcp any any -> 104.21.55.70 80 (msg:"WarmCookie ZIP download detected"; flow:to_server,established; content:"GET"; http_method; content:"/managements?"; http_uri; sid:300001; rev:1;)
```
- **Purpose**: Detects the download of a ZIP file from the domain `quote.checkfedexexp.com`.
- **Trigger Condition**:
  - A GET request to IP `104.21.55.70` on port 80 with the URI `/managements?`.
- **Explanation**:
  - This activity is associated with the initial delivery of the WarmCookie malware. Monitoring such downloads helps prevent the infection from propagating.

---

### 2. **WarmCookie Follow-Up Download Detected**
```snort
alert tcp any any -> 172.67.170.169 443 (msg:"WarmCookie follow-up download detected"; flow:to_server,established; content:"Host|3A| business.checkfedexexp.com"; http_header; sid:300002; rev:1;)
```
- **Purpose**: Detects HTTPS traffic to the malicious domain `business.checkfedexexp.com`.
- **Trigger Condition**:
  - Any HTTPS request containing the `Host` header `business.checkfedexexp.com`.
- **Explanation**:
  - This rule identifies follow-up communication or downloads associated with WarmCookie, which often involves accessing this domain for secondary payloads.

---

### 3. **WarmCookie DLL Download Detected**
```snort
alert tcp any any -> 72.5.43.29 80 (msg:"WarmCookie DLL download detected"; flow:to_server,established; content:"GET"; http_method; content:"/data/0f60a3e7baecf2748b1c8183ed37d1e4"; http_uri; sid:300003; rev:1;)
```
- **Purpose**: Detects the download of a malicious DLL from IP `72.5.43.29`.
- **Trigger Condition**:
  - A GET request to IP `72.5.43.29` on port 80 with the URI `/data/0f60a3e7baecf2748b1c8183ed37d1e4`.
- **Explanation**:
  - This rule targets the retrieval of a known malicious DLL file, which is a part of the malware’s functionality.

---

### 4. **WarmCookie POST-Infection Traffic Detected**
```snort
alert tcp any any -> 72.5.43.29 80 (msg:"WarmCookie POST-infection traffic detected"; flow:to_server,established; content:"POST"; http_method; sid:300004; rev:1;)
```
- **Purpose**: Detects suspicious HTTP POST traffic to the WarmCookie server at IP `72.5.43.29`.
- **Trigger Condition**:
  - Any POST request to IP `72.5.43.29` on port 80.
- **Explanation**:
  - POST requests are commonly used for exfiltrating data or sending commands, making this rule vital for identifying post-infection activity.

---

### 5. **WarmCookie GET-Infection Traffic Detected**
```snort
alert tcp any any -> 72.5.43.29 80 (msg:"WarmCookie GET-infection traffic detected"; flow:to_server,established; content:"GET"; http_method; sid:300005; rev:1;)
```
- **Purpose**: Detects suspicious HTTP GET traffic to the WarmCookie server at IP `72.5.43.29`.
- **Trigger Condition**:
  - Any GET request to IP `72.5.43.29` on port 80.
- **Explanation**:
  - Monitoring GET requests helps identify communication patterns and potential C2 traffic used by WarmCookie.

---

### 6. **Suspicious Activity from Infected Host**
```snort
alert ip 10.8.15.133 any -> any any (msg:"Suspicious activity from infected host (10.8.15.133)"; sid:300006; rev:1;)
```
- **Purpose**: Monitors all traffic originating from the compromised host `10.8.15.133`.
- **Trigger Condition**:
  - Any IP traffic from `10.8.15.133`.
- **Explanation**:
  - This rule captures any unusual activity from the infected device, which can include lateral movement or C2 communication.

---

## Why These Rules Are Usable

1. **Precise Targeting**:
   - Rules are based on specific IOCs such as IPs, URIs, and domains linked to WarmCookie activities.

2. **Behavioral and Content-Based Detection**:
   - Each rule identifies behaviors like suspicious file downloads or C2 communication patterns.

3. **Comprehensive Coverage**:
   - These rules cover initial infection, follow-up downloads, and post-infection activities, ensuring a wide detection scope.

---

## When Each Rule Triggers

1. **ZIP Download**:
   - Triggered by a GET request for `/managements?` from `quote.checkfedexexp.com`.

2. **HTTPS Traffic**:
   - Triggered by HTTPS requests to `business.checkfedexexp.com`.

3. **DLL Download**:
   - Triggered by a GET request for `/data/0f60a3e7baecf2748b1c8183ed37d1e4` from `72.5.43.29`.

4. **POST Traffic**:
   - Triggered by any POST request to `72.5.43.29`.

5. **GET Traffic**:
   - Triggered by any GET request to `72.5.43.29`.

6. **Infected Host Activity**:
   - Triggered by any IP traffic originating from `10.8.15.133`.

---

## Summary

These rules provide targeted detection for WarmCookie malware activities, from initial infection to post-infection behavior. By monitoring key communication patterns and compromised devices, these rules enable effective identification and response to WarmCookie threats.

