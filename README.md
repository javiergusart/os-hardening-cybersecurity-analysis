# Operating System Hardening and Security Analysis

## Table of Contents
1. [Overview](#overview)
2. [Section 1: Identify the Network Protocol Involved in the Incident](#section-1-identify-the-network-protocol-involved-in-the-incident)
   - [Network Protocol Chart](#network-protocol-chart)
3. [Section 2: Document the Incident](#section-2-document-the-incident)
   - [Incident Details](#incident-details)
   - [Analysis and Investigation](#analysis-and-investigation)
   - [Incident Timeline Chart](#incident-timeline-chart)
   - [Key Findings](#key-findings)
4. [Section 3: Recommendations to Prevent Brute Force Attacks](#section-3-recommendations-to-prevent-brute-force-attacks)
   - [Prevention Chart](#prevention-chart)
5. [Conclusion](#conclusion)

## Overview
This activity report examines a brute force attack that compromised a web application through unauthorized access. As part of the **Google Cybersecurity Certification**, specifically within the course **Connect and Protect: Networks and Network Security**, the primary objective is to illustrate how OS hardening techniques and network security principles can effectively mitigate the risks posed by brute force attacks.

In this incident, a former employee exploited a weak default password to gain access to the admin panel of the cooking website, `yummyrecipesforme.com`. Upon gaining entry, the attacker modified the source code to inject a malicious JavaScript file that prompted unsuspecting visitors to download malware. This malware subsequently redirected users to a fraudulent site, `greatrecipesforme.com`, further compromising their systems. 

> **Note**: The websites referenced in this report, `yummyrecipesforme.com` and `greatrecipesforme.com`, are fictitious and used for illustrative purposes only.

To investigate this incident, I used **tcpdump**, a robust network packet analyzer, to monitor the attack and trace the propagation of the malware. My findings will detail the nature of the attack and provide recommendations for implementing **OS hardening techniques** to prevent similar incidents in the future.

Additionally, I reference several key resources that enhance our understanding of the protocols and techniques discussed throughout the report, including:

- **[Tcpdump Traffic Log](https://docs.google.com/document/d/1ShBplKI8pUqhYKxBVT-_UNXJ8CgGu2CGImezD4LUh9k/edit?usp=sharing)**: A resource that provides sample logs for analysis.
- **[How to Read the Tcpdump Traffic Log](https://docs.google.com/document/d/1SifoqPD-zryiZkT6GOtBTGkkfoECdkAIoaJ0JA62-XA/edit?tab=t.0#heading=h.shz1bcdh2tm3)**: A guide that offers insights into interpreting network traffic data.

## Section 1: Identify the Network Protocol Involved in the Incident

The incident primarily involved two network protocols: **DNS (Domain Name System)** and **HTTP (Hypertext Transfer Protocol)**. These are the protocols that enabled communication between the browser and the compromised web application. 

- **DNS** resolved the domain name into its corresponding IP address.
- **HTTP** managed the transfer of web content, including the malicious file.

### Network Protocol Chart

| Protocol | Role in the Incident                                | Port   |
|----------|-----------------------------------------------------|--------|
| DNS      | Resolved the IP addresses of the domains            | 53     |
| HTTP     | Facilitated the transfer of the website and malware | 80     |

## Section 2: Document the Incident

### Incident Details
The security event was triggered when users reported that the fictitious website, `yummyrecipesforme.com`, prompted them to download a file containing "free recipes." After running the file, their computers slowed down, and their browsers were redirected to a fake site, `greatrecipesforme.com`.

The **tcpdump** logs show the process as follows:
1. A **DNS request** to resolve the domain.
2. An **HTTP GET request** to load the website.
3. The site prompted a download of the malicious file.
4. After downloading and running the file, another **DNS request** was made to redirect users to the malicious site.

### Analysis and Investigation
Using a **sandbox environment** (an isolated virtual environment that prevents malware from spreading), we analyzed the downloaded file and observed the browser’s redirection.

The logs also revealed how the malware redirected the browser:
```plaintext
14:20:32.192571 IP your.machine.52444 > dns.google.domain: 21899+ A? greatrecipesforme.com. (24)
14:20:32.204388 IP dns.google.domain > your.machine.52444: 21899 1/0/0 A 192.0.2.17 (40)
14:25:29.576493 IP your.machine.56378 > greatrecipesforme.com.http: Flags [S], seq 1020702883, win 65495, options [mss 65495,sackOK,TS val 3302989649 ecr 0,nop,wscale 7], length 0
```

### Incident Timeline Chart

| Step                                | Action/Observation                                                             |
|-------------------------------------|--------------------------------------------------------------------------------|
| **DNS Request for Domain**         | The browser sends a DNS request to resolve the domain name.                   |
| **HTTP GET Request**                | The browser sends an HTTP GET request to the resolved IP address.             |
| **Prompt for Malicious Download**   | The website prompts the user to download a malicious file disguised as a browser update. |
| **Malware Download and Execution**  | The user downloads and executes the malicious file, compromising their system. |
| **DNS Request for Fake Site**      | The browser sends a DNS request to resolve the malicious site after executing the malware. |
| **Redirection to Malicious Site**   | The user is redirected to a fake website containing additional malware.       |

### Key Findings
- The attacker exploited a **default admin password** to gain unauthorized access.
- Once inside the admin panel, they altered the source code to distribute malware to visitors.
- The **tcpdump** logs confirmed the transition from legitimate HTTP traffic to malicious HTTP traffic after the malware was executed.

## Section 3: Recommendations to Prevent Brute Force Attacks

To prevent similar attacks in the future, we recommend implementing several OS hardening techniques, as well as security controls that specifically address brute force attacks:

### 1. Disallow Default or Reused Passwords
Attackers often exploit default or weak passwords. Organizations must enforce policies that prevent the reuse of old or default passwords.

### 2. Enforce Frequent Password Updates
Regular password changes reduce the time attackers have to exploit compromised credentials. Forcing frequent updates can mitigate this risk.

### 3. Implement Multi-Factor Authentication (MFA)
MFA adds a second layer of security. Even if an attacker guesses the password, they would need an additional factor (such as a one-time password sent via email or SMS) to gain access.

### 4. Utilize CAPTCHA for Login Pages
Implementing CAPTCHA or reCAPTCHA can prevent automated bots from making repeated login attempts, reducing the effectiveness of brute force attacks.

### 5. OS Hardening
Hardening the operating system involves:
- Disabling unused services.
- Securing default accounts.
- Regularly applying security patches.

These practices help reduce the number of exploitable vulnerabilities on the server.

### Prevention Chart

| Recommendation                 | Benefit                                                                                     |
|---------------------------------|---------------------------------------------------------------------------------------------|
| Disallow Default/Old Passwords  | Prevents reuse of compromised passwords, reducing brute force attack success rate           |
| Frequent Password Updates       | Limits attack window by forcing password changes on a regular basis                         |
| Multi-Factor Authentication     | Adds a second layer of security, making password guessing insufficient to gain access       |
| CAPTCHA on Login Pages          | Prevents automated brute force attempts by requiring human verification                     |
| OS Hardening                    | Reduces vulnerabilities by securing system configurations and removing unnecessary services |

## Conclusion

Reflecting on this brute force attack, I've gained valuable insights into the vulnerabilities that can exist in web applications and the critical importance of maintaining strong security measures. This incident underscored the necessity of robust password policies, as relying on default passwords can lead to severe consequences. Organizations should encourage the use of complex, unique passwords and regular updates to ensure ongoing security.

The experience reinforced the value of continuous security audits and proactive vulnerability assessments, which can significantly strengthen defenses. Implementing multi-factor authentication is essential, as it adds an important layer of security that helps mitigate risks even if a password is compromised.

Additionally, utilizing tools like **tcpdump** has highlighted the significance of monitoring network traffic to identify potential threats and respond quickly to incidents. Overall, this experience has deepened my understanding of cybersecurity and the proactive measures necessary to protect against future threats.
