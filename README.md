# Web Investigation Challenge: Network Analysis Using Wireshark and Network Miner https://cyberdefenders.org/blueteam-ctf-challenges/web-investigation/

## Project Overview
In this network forensics analysis, I investigated a simulated SQL injection attack using Wireshark and Network Miner. This analysis covers each step taken to identify and investigate the attacker’s IP address, trace their actions, and uncover the details of the attack.

---

## Table of Contents
- [Objectives](#objectives)
- [Tools Used](#tools-used)
- [Analysis](#analysis)
  - [Q1. Identifying Attacker’s IP](#q1-identifying-attackers-ip)
  - [Q2. Determining Origin City of Attacker](#q2-determining-origin-city-of-attacker)
  - [Q3. Identifying Exploited Script](#q3-identifying-exploited-script)
  - [Q4. Finding the URI of the First SQLi Attempt](#q4-finding-the-uri-of-the-first-sqli-attempt)
  - [Q5. Extracting Complete URI Requests](#q5-extracting-complete-uri-requests)
  - [Q6. Database and Table Information](#q6-database-and-table-information)
  - [Q7. Confirming Admin Access](#q7-confirming-admin-access)
  - [Q8. Tracking Login Attempts](#q8-tracking-login-attempts)
  - [Q9. Identifying Malicious Script](#q9-identifying-malicious-script)
- [References](#references)
- [Appendix](#appendix)

---

## Objectives
1. To identify and analyze the IP address of the attacker.
2. To trace the origin of the attack.
3. To find out how the SQL injection was executed and what resources were compromised.

## Tools Used
- **Wireshark**: For capturing and analyzing network traffic.
- **Network Miner**: To extract artifacts from network traffic and uncover malicious activities.
- **Cisco Talos Intelligence**: For additional threat intelligence on suspicious IP addresses.

---

## Analysis

### Q1. Identifying Attacker’s IP
*Fig.1: Identifying attacker’s IP address in Wireshark*(https://github.com/Amara-001/Network-Analysis/Attacker's IP address.png)


Using Wireshark, I filtered for `GET` requests to identify the attacker’s IP address, which is responsible for sending SQL injection payloads. The identified IP is **111.224.250.131**.


### Q2. Determining Origin City of Attacker
*Fig. 2: Cisco Talos Intelligence reveals attacker’s IP origin*![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)

Using Cisco Talos Intelligence, I performed threat intelligence on the IP address **111.224.250.131**, which revealed the origin city of the attack to be Shijiazhuang, China

### Q3. Identifying Exploited Script
The attacker targeted `search.php` for SQL injection, as shown in requests containing SQLi payloads like `search.php?search=<SQLI PAYLOAD>`. This vulnerability in `search.php` allowed for unauthorized access.

### Q4. Finding the URI of the First SQLi Attempt
I searched for the SQLi payload (`1=1`) against the attacker’s IP address. The URI of the first `GET` request was captured to identify the initial SQL injection attempt.

### Q5. Extracting Complete URI Requests
I filtered the attacker's IP address against HTTP response code `200` and used the `mysql` packet detail to expand the URI request, uncovering the full payload.
![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)
![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)


### Q6. Database and Table Information
Filtered for packets with response code `200` related to `bookword_db`, revealing a database structure. The table targeted was `customers`.
![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)
![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)


### Q7. Confirming Admin Access
Using a filter for `admin` in HTTP response code `200`, I traced the attacker’s path to the admin directory, confirming they gained admin access.

*Fig. 3: Attacker gains access to the admin panel.*![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)
![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)
![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)

### Q8. Tracking Login Attempts
By filtering the attacker’s IP and examining `POST` requests in Wireshark, I identified multiple login attempts. The attacker succeeded on the fourth attempt.
![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)
![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)

### Q9. Identifying Malicious Script
I searched `http.request.method==POST` to locate the malicious script (x-php) in the requests. It was embedded within `MIME Multipart Media Encapsulation` and found within the POST request packets.
![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)
![Attacker IP Identification](https://github.com/your-username/your-repo-name/blob/main/path-to-image/filename.png)


---

## References
- **Awati, Rahul**. [What is a Uniform Resource Identifier (URI)?](https://www.techtarget.com/whatis/definition/URI-Uniform-Resource-Identifier). Accessed May 25, 2024.
- **Cisco Talos Intelligence**. [About Cisco Talos](https://www.talosintelligence.com/about). Accessed May 25, 2024.

---

## Appendix
1. **CyberDefenders**: A training platform for SOC analysts, threat hunters, and DFIR experts to develop defensive skills.
2. **Wireshark**: An open-source network protocol analyzer for detailed packet analysis and real-time traffic monitoring.
3. **Network Miner**: A forensic tool for extracting artifacts like images, passwords, and emails from captured network traffic.
4. **Cisco Talos**: A cybersecurity platform that provides threat intelligence to defend against known and emerging threats.

---

This project highlights practical network analysis techniques and investigative steps for tracing and mitigating malicious cyber activity.

