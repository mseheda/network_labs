
# Snort Rule Analysis Report

## Overview
This report consolidates the Snort rules and corresponding outputs for three different malware detection tasks. Each task contains:
- A description of the rules created.
- A hyperlink to download the rules in `.md` format.
- A hyperlink to the `.txt` output file of Snort when processing the respective rules.
- Screenshots of the results.

---

## Tasks

### Task 1: NetSupport RAT and SmartApeSG Detection
- **Description**:
  - Rules detect traffic associated with the NetSupport RAT C2 server, malicious domains (`modandcrackedapk.com`), and compromised host activity.
- **Files**:
  - [NetSupport RAT Rules](sandbox:/mnt/data/Snort_Rule_Report.md)
  - [NetSupport RAT Snort Output](sandbox:/mnt/data/1st-out.txt)

---

### Task 2: Koi Stealer Detection
- **Description**:
  - Rules detect Koi Stealer C2 communication, suspicious traffic to malicious domains, and compromised host activity.
- **Files**:
  - [Koi Stealer Rules](sandbox:/mnt/data/Snort_Koi_Stealer_Report.md)
  - [Koi Stealer Snort Output](sandbox:/mnt/data/snort_outpu2t.txt)

---

### Task 3: WarmCookie Malware Detection
- **Description**:
  - Rules identify WarmCookie malware activities, including malicious ZIP and DLL downloads, suspicious traffic to malicious servers, and compromised host monitoring.
- **Files**:
  - [WarmCookie Rules](sandbox:/mnt/data/Snort_WarmCookie_Report.md)
  - [WarmCookie Snort Output](sandbox:/mnt/data/warmcookie-output.txt)

---

## Screenshots

### Screenshot 1
![Task 1 and Task 2 Results](sandbox:/mnt/data/1-and-2.png)

### Screenshot 2
![Task 2 and Task 3 Results](sandbox:/mnt/data/2-and-3.png)

---

## Definition of Done

1. Snort rules created and validated for each task.
2. `.txt` output files generated after processing traffic with Snort for each task.
3. Clear Markdown documentation created for each task, including descriptions and hyperlinks.
4. Consolidated main `.md` file with hyperlinks to task-specific files and screenshots.

