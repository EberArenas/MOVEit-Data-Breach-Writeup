# MOVEit-Data-Breach
Writeup for MOVEit data breach

Final Project ISM4323

	The MOVEit data breach occurred in the summer of 2023 when a wave of cyberattacks and data breaches were discovered in MOVEit, a file transfer software widely used to transfer sensitive data inside the healthcare industry. Not only that, but banks, government agencies, as well as thousands of IT departments handling sensitive data may have been implicated in the attack. Released in 2002, MOVEit (a product of Ipswitch which is itself a subsidiary of Progress Software) has long integrated antivirus software to secure its databases from hackers as well as ensuring security via protocols such as encrypting files and using file transfer protocols such as FTP, FTPS, and SFTP to transfer its data. As a small aside, they also introduced their cloud-based transfer system in 2012. These security features ensured that MOVEit would be used as a trusted worldwide file transfer system, and which is also why the 2023 data breach came as a complete surprise to the world and was quickly dubbed as the largest hack of the year. Starting over the Memorial Day Holiday of 2023, the zero-day vulnerability was announced by Progress Software and was subsequently assigned as CVE-2023-34362. Further investigations by Mandiant revealed more evidence that exploitation began as soon as May 27th, 2023, just two days prior to the announcement. In a surprising turn of events, all indications signaled that the incident itself turned out to be a SQL injection. Some other indicators of compromise that were revealed were web shells, with some data theft occurring just minutes after the web shell deployments. However, there were no immediate threats for ransom that usually occur in these sorts of attacks. It wasn’t until June 6th that an announcement was finally made in a blog post by the group known as Clop, which also made announcements to ask for an extortion fee in return for not releasing the documents. In this growing digital age, organizations are increasingly relying on third-party software to perform day to day tasks and streamline operation. While this is great for boosting efficiency (and not being bogged down in ‘reinventing the wheel’) this also expands the attack surface, since the code and protocols within are not regulated by the organizations themselves and thus there’s no guarantee that they are adhering to security best practices. This makes software vulnerabilities a high risk, and high profile breaches such as the MOVEit breach highlights the need for rigorous security protocols to be put in place to thwart preventable incidents such as these and to protect the general public. Thus, this paper will analyze the MOVEit data breach of 2023 to focus on why it occurred, what steps could have been taken to prevent it, and what similar organizations could do to prevent such attacks in the future. 
To start off, we will detail the incident and how it occurred. Some of the basics were already shared in the introduction, but to elaborate: In 2023 the attack seems to have occurred as early as May 27th, 2023, where, through the use of privilege escalation and arbitrary code execution, a series of LEMURLOOT webshells were hosted to aid in the exfiltration of file data. LEMURLOOT is a webshell written in C# tailor made to interact with the MOVEit transfer program. It can run commands to download files from the MOVEit Transfer system, extract Azure system settings, retrieve detailed record information, as well as create, insert, or delete a particular user. All data returned to LEMURLOOT is gzip compressed. The main component being checked by LEMURLOOT is if the incoming HTTP request contains the header field X-siLock-Comment. If the parameters are correct, the webshell sends a header response X-siLock-Comment and connects to a SQL server from the executing host. From there, the malware takes certain steps depending on the value of the header field. For example, it can delete a user with LoginName and RealName set to “Health Check Service” using the following SQL command (provided by Mandiant, 2023): 

Delete FROM users WHERE RealName='Health Check Service'

It seems that the success of the attack was in part based on what database engine was used (such as MySQL, Microsoft SQL Server, or Azure SQL), as LEMURLOOT was singled out to also steal from Azure Storage Blob information from MOVEit. This indicates that the threat actor was also stealing files from Azure when users were using items from Azure Storage Blob, but it’s unclear if the damage extended from there. On May 28th, various samples of LEMURLOOT (specifically, a file named human2.aspx in the root directory of vulnerable servers. While not necessary to run the exploit, the file was deployed to enable persistence) were uploaded to VirusTotal, and in the following months samples also began to crop up in various countries such as Pakistan and Germany, suggesting that other countries were also targeted. Although the vulnerability was quickly patched within four days of the earliest attacks, the damage was unfortunately already done as almost 100 million individuals were already affected in the MOVEit data breach.
Given how the incident occurred, what went wrong, and how many people were affected, it’s important to take into strong consideration the measures that could have been taken to avoid the incident from occurring in the first place. With the amount of sensitive information running through the file transfer system, it is their legal, moral, and professional duty to ensure that the company is making their best effort to adhere to the best available information security practices. As it stands, the fact that such a large breach was allowed to occur through an SQL injection vulnerability in 2023 is disconcerting. While not a catch-all, more rigorous and proactive penetration testing and code reviews could have been put in place to potentially catch this zero-day vulnerability and related ones internally before any harm could have occurred. However, as the book “97 Things Every Information Security Professional Should Know: Collective Wisdom from the Experts” helpfully adds, penetration testing is not just about finding the exploit and going home. Proper reporting and documentation are needed to keep track of the vulnerability as news of it is spread within the organization and outside of it. As per Mandiant, some immediate remediation strategies would be to try to isolate the impacted servers, implement patches, and investigate the incident. Isolate servers by disabling inbound and outbound communication by editing firewall rules to block TCP ports 80 and 443, then apply patches provided by Progress. Clearing active sessions, rotating credentials, and enabling MFA are all paramount before reconstituting the application back into production. Investigating the incident would involve hunting known IOC’s of the company, such as webshell files (human2.aspx) or certain IP addresses, to better identify vulnerabilities within local networks. Continuous monitoring of the network, endpoints, and logs for these IOCs is also recommended. Information revealed from investigations can be used to better inform cybersecurity organizations on how the attack occurred and how their incident response plans and drills could be updated to better react against such threats. Moving forward to plan ahead, it’s essential to harden systems to ensure that such attacks don’t succeed in the future. On top of enabling MFA for accounts using the MOVEit transfer application, local data encryption keys ought to be rotated to re-encrypt local filestores and MOVEit applications configured with Azure Storage ought to be configured to either periodically rotate account access keys or to configure Azure firewall rules to allow only a limited set of trusted IP addresses. Best practices include updating remote access policies to only allow inbound connections from trusted IPs and to allow inbound access from trusted entities. Additional countermeasures can include reviewing and editing account permissions in accordance with principles of least privilege, adopting Managed EDR (such as Huntress’s offer in the following month of the attack), and editing Windows Group Policy Object configurations to prevent lateral movement tactics by threat actors. On the topic of editing account permissions, privileged accounts should only be accessed through certain devices, and should not be able to be remotely accessed nor should they have direct email and web access. Again, this is where the book “97 Things” has some helpful advice: in order to best implement any of these changes, it’s important that we understand why the breach happened in the first place. According to Christina Morillo: “Focus on the what and the why first, not the tool.” The solution may not always be to buy the shiniest new tool on the market, but to be introspective and evaluate all sides of the problem. Maybe it’s not so much an issue with the tools themselves, but with the organization’s structure, or maybe it lacked a sufficient incident response plan. 
To briefly recap, the MOVEit data breach of 2023 was one of the largest data breaches of the year with widespread consequences to companies big and small alike. There’s a pressing need for companies to ensure their code aligns with federal standards and best practices set by NIST and the like, as there can be dire downstream consequences both for the individual company and for society at large. 
References:

Hammond, J. (2023, June 1). MOVEit Transfer Critical Vulnerability CVE-2023-34362 Rapid Response. Www.huntress.com. https://www.huntress.com/blog/moveit-transfer-critical-vulnerability-rapid-response

Mandiant. (2023, June 2). Zero-Day Vulnerability in MOVEit Transfer Exploited for Data Theft | Mandiant. Google Cloud Blog; Google Cloud. https://cloud.google.com/blog/topics/threat-intelligence/zero-day-moveit-data-theft/
‌
MOVEit Transfer: Containment and Hardening Guide V1. 7 -JULY 06, 2023 MANDIANT. (n.d.). Retrieved December 11, 2024, from https://services.google.com/fh/files/misc/moveit-containment-hardening-guide-rpt-en.pdf

Newman, L. H., & Burgess, M. (2023, October 2). The Biggest Hack of 2023 Keeps Getting Bigger. Wired. https://www.wired.com/story/moveit-breach-victims/

NIST. (2023, June 2). NVD - CVE-2023-34362. Nvd.nist.gov. https://nvd.nist.gov/vuln/detail/CVE-2023-34362

Page, C. (2023, August 25). MOVEit, the biggest hack of the year, by the numbers. TechCrunch. https://techcrunch.com/2023/08/25/moveit-mass-hack-by-the-numbers/

Progress Community. (2023, June 16). Progress Customer Community. Community.progress.com. https://community.progress.com/s/article/MOVEit-Transfer-Critical-Vulnerability-31May2023

