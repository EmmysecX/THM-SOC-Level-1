# Boogeyman 3

![Boogeyman3](assets3/Boogeyman3.png)
The Boogeyman emerges from the darkness again.

## TASK 1: Introduction
Due to the previous attacks of Boogeyman, Quick Logistics LLC hired a managed security service provider to handle its Security Operations Center. Little did they know, the Boogeyman was still lurking and waiting for the right moment to return. 

## TASK 2: The Chaos Inside

Lurking in the Dark

Without tripping any security defences of Quick Logistics LLC, the Boogeyman was able to compromise one of the employees and stayed in the dark, waiting for the right moment to continue the attack. Using this initial email access, the threat actors attempted to expand the impact by targeting the CEO, Evan Hutchinson.

Initial Investigation

Upon receiving the phishing email report, the security team investigated the workstation of the CEO. During this activity, the team discovered the email attachment in the downloads folder of the victim.

Lastly, it was presumed by the security team that the incident occurred between August 29 and August 30, 2023.

Given the initial findings, you are tasked to analyse and assess the impact of the compromise.

**Answer the questions below**

**Q2.1 What is the PID of the process that executed the initial stage 1 payload?**

Firstly, I changed the date to the day the incident happened. 

![date of incident](assets3/Task2_screenshot1a.png)

Then I filtered the logs with sysmon event ID 1 (process creation) and it was sent as a pdf file. From the output, I saw a pdf.hta file, so I expanded it to check for the PID.

![PID](assets3/Task2_screenshot1b.png)

> **ANS:** 6392

**Q2.2 The stage 1 payload attempted to implant a file to another location. What is the full command-line value of this execution?**

Since I already have the process that executed the payload, I added the process.commandline and the process.parent as columns to check the commands the process is involved with.

![command-line value](assets3/Task2_screenshot2.png)

> **ANS:** `"C:\Windows\System32\xcopy.exe" /s /i /e /h D:\review.dat C:\Users\EVAN~1.HUT\AppData\Local\Temp\review.dat`

**Q2.3 The implanted file was eventually used and executed by the stage 1 payload. What is the full command-line value of this execution?**

The implanted file was review.dat, so I saw another process already running that.

![implanted file command-line value](assets3/Task2_screenshot3.png)

> **ANS:** `"C:\Windows\System32\rundll32.exe" D:\review.dat,DllRegisterServer`

**Q2.4 The stage 1 payload established a persistence mechanism. What is the name of the scheduled task created by the malicious script?**

I searched for the file name and looked at the PowerShell command for any scheduled task.

![scheduled task created by the malicious script](assets3/Task2_screenshot4.png)

> **ANS:** Review

**Q2.5 The execution of the implanted file inside the machine has initiated a potential C2 connection. What is the IP and port used by this connection? (format: IP:port)**

I filtered my search with the username, which I found earlier, and also filtered it with powershell.exe. I added the destination IP and port as a field, and I saw a particular IP address in a lot of instances.

![IP and port](assets3/Task2_screenshot5.png)

> **ANS:** 165.232.170.151:80

**Q2.6 The attacker has discovered that the current access is a local administrator. What is the name of the process used by the attacker to execute a UAC bypass?**

I Googled common UAC bypass techniques, then I filtered the process name search on Kibana.

![process used by the attacker to execute a UAC bypass](assets3/Task2_screenshot6.png)

> **ANS:** fodhelper.exe

**Q2.7 Having a high privilege machine access, the attacker attempted to dump the credentials inside the machine. What is the GitHub link used by the attacker to download a tool for credential dumping?**

I searched for "*git*" and checked the process.commandline. I found the GitHub link.

![GitHub link used by the attacker to download a tool for credential dumping](assets3/Task2_screenshot7.png)

> **ANS:** hxxps://github[.]com/gentilkiwi/mimikatz/releases/download/2.2.0-20220919/mimikatz_trunk.zip (Defanged URL)

**Q2.8 After successfully dumping the credentials inside the machine, the attacker used the credentials to gain access to another machine. What is the username and hash of the new credential pair? (format: username:hash)**

I searched for *ntlm* and got the answer.

![username and hash of the new credential pair](assets3/Task2_screenshot8.png)

> **ANS:** itadmin:F84769D250EB95EB2D7D8B4A1C5613F2

**Q2.9 Using the new credentials, the attacker attempted to enumerate accessible file shares. What is the name of the file accessed by the attacker from a remote share?**

For the attacker to enumerate accessible file shares, he has to use a tool. I searched for "git" to see if there was another tool downloaded apart from mimikatz. I found PowerView.ps1, which was used to invoke share-finder.

![tool](assets3/Task2_screenshot9a.png)

Then I expanded the row but didn't find what I was looking for, so I added the agent hostname to my filter and searched for a "*ps1*" file. I found the path with a file.

![file accessed by the attacker from a remote share](assets3/Task2_screenshot9b.png)

> **ANS:** IT_Automation.ps1

**Q2.10 After getting the contents of the remote file, the attacker used the new credentials to move laterally. What is the new set of credentials discovered by the attacker? (format: username:password)**

I searched for *credential* if I found anything useful. I saw a lot of dashes in the process.command line, so I excluded it from the filter. Then I found the new credentials.

![new set of credentials](assets3/Task2_screenshot10.png)

> **ANS:** QUICKLOGISTICS\allan.smith:Tr!ckyP@ssw0rd987

**Q2.11 What is the hostname of the attacker's target machine for its lateral movement attempt?**

I filtered the username to allan.smith and checked the agent.hostname.

![hostname](assets3/Task2_screenshot11.png)

> **ANS:** WKSTN-1327

**Q2.12 Using the malicious command executed by the attacker from the first machine to move laterally, what is the parent process name of the malicious command executed on the second compromised machine?**

Since I have the hostname, I searched for it and filtered for event.code 1 (process creation). I arranged the time in ascending order to see where the processes started from. I saw the attacker’s commands, and my focus was the processes that came after the credential command because that is when he accessed the second machine. I found wsmprovhost.exe, which is a PowerShell remote execution.

![parent process name of the malicious command executed on the second compromised machine](assets3/Task2_screenshot12.png)

> **ANS:** wsmprovhost.exe

**Q2.13 The attacker then dumped the hashes in this second machine. What is the username and hash of the newly dumped credentials? (format: username:hash)**

I kept scrolling down the output and found the dumped credentials for the administrator account.

![username of newly dumped credentials](assets3/Task2_screenshot13.png)

> **ANS:** administrator:00f80f2538dcb54e7adc715c0e7091ec

**Q2.14 After gaining access to the domain controller, the attacker attempted to dump the hashes via a DCSync attack. Aside from the administrator account, what account did the attacker dump?**

I searched for "*dcsync*" and found the other user.

![other account dumped](assets3/Task2_screenshot14.png)

> **ANS:** backupda

**Q2.15 After dumping the hashes, the attacker attempted to download another remote file to execute ransomware. What is the link used by the attacker to download the ransomware binary?**

I filtered the process name to PowerShell and searched the command line. I saw a link with the name ransomboogey.exe.

![link used by the attacker to download the ransomware binary](assets3/Task2_screenshot15.png)

> **ANS:** hxxp[:]//ff[.]sillytechninja[.]io/ransomboogey[.]exe

## Conclusion

Why did I search for ntlm in question 2.8? NTLM (NT LAN Manager) is a protocol used by Windows systems for authentication. When an attacker successfully dumps credentials from a compromised machine, those credentials may be stored in the form of NTLM hashes rather than plaintext passwords. These hashes are used in authentication processes on Windows systems.

In this task, we successfully traced the Boogeyman's steps from the initial phishing attack to the final stages of credential dumping, lateral movement, and ransomware deployment. By analyzing key logs and system events, we were able to identify the tools and techniques used by the attacker, including UAC bypass, credential dumping, and DCSync attacks. This exercise highlights the importance of monitoring system events, tracking command-line activity, and analyzing network traffic to detect and mitigate advanced persistent threats in a timely manner.

