[home/](../../../)[blogs+posts/](../../)[software-engineering/](../)[endpoint-protection](./)


# A Short History of Antimalware

<img src="https://c.pxhere.com/photos/5f/b9/computer_gaming_green_keyboard_microsoft_pc_technology_typing-916984.jpg!d" srcset="https://c.pxhere.com/photos/5f/b9/computer_gaming_green_keyboard_microsoft_pc_technology_typing-916984.jpg!d" width="1000" height="400">

_Photo Credit: [pxhere.com](https://pxhere.com/en/photo/916984)_


### Introduction
Most people don't realize how many discrete technologies go into modern-day endpoint protection solutions. The term _Antivirus_ or AV has become overloaded and arguably, doesn't quite describe the array of protection, detection and correction measures deployed to your device. More often within cybersecurity circles, _Antivirus_ really just means traditional, signature-based antivirus protection, which scans, detects, quarantines and removes known malware from your host. There is often a remediation aspect to AV, which involves 'cleaning' or undoing the effects of the known malware. Remediation is another overloaded term so in this instance, this form of remediation is often referred to as _basic remediation_. 

In all, modern Endpoint Protection (EP) represents a collection of technologies added over time that have addressed threats as they've emerged. In many ways, _Antimalware_ is demand-led by _Malware_ authors. For instance, Ransomware really only became something of interest to Threat Researchers circa 2015 but hit news headlines repeatedly during 2016 as various versions of Ransomware (e.g. WannaCry, Petya, NotPetya) created havoc around the globe. Cybersecurity vendors reacted by adding anti-ransomware protection and anti-ransomware remediation features to their products.

The point is that modern solutions are a collection of technologies developed over time as the threat landscape has evolved. While there are some that regard Endpoint Protection as a commodity, the fact is small differences in _Efficacy_ (a measure of the extent to which antimalware works successfully), can mean the difference in being protected or being vulnerable to, for example, _Zero Day_ threats. 

## The Evolution of Antimalware 

In a sense, the Antimalware industry was born pretty much right after the idea of self-reproducing software (or virus) came to light. In this context, the term _virus_ merely meant that a piece of software could replicate and distribute copies of itself, not that it was necessarily malicious. However, it wasn't long before malicious software, either intentionally or unintentionally, emerged and the term _virus_ became synonymous with malicious software - or _malware_. In particular, towards the end of the '80s, there was a rise in malicious viruses targeting IBM PCs.

### Pre-1990's
Early scan solutions to detect known viruses and remove them were developed, though these were specifically written to target known viruses. In the days before the internet became commonplace, viruses were more likely spread by sharing infected floppy disks. AV software was distributed similarly and was often installed after the fact to detect & clean. This meant doing a on-demand scan (ODS) of your computer. 

### 1990's
This was the decade that witnessed the PC and Client/Server revolution so it should come as no surprise that it was also the period that saw a significant increase in the number and complexity of malware. Companies like McAfee and Norton emerged offering solutions initially based on hand-generated signature, then later automated hash-based signature detection but, by the end of the decade, had shifted to more advanced-heuristic methods in order to detect malware at scale. 

In the early days, signatures were hand-written to exactly identify a virus instance. The AV engines understood file formats and the signature contained instructions on what to look for. Thus began the game of cat-and-mouse played between malware authors and antimalware vendors, which involved narrowing the time-period from the point the virus was released to the wild to the time AV companies could generate and issue a new signature to detect it. Malware authors tried to evade detection by making the virus code more complex and thus harder to generate a signature for it or using polymorphic code to constantly generate new versions of itself, albeit only slightly altered. Malware authors also tried to widen the field by using other types of files, other than executables, such as Viruses for Word Documents, Office macros, VBA Scripts, etc. Both executable malware and macro viruses became 'parasitic', which means they attached themselves to genuine executables and macros in order to evade detection. 

The overarching problem though, was one of scale. The rate at which new malware samples emerged overwhelmed AV vendors ability to address it. There were humans in the system writing signatures and there was simply too much malware to analyze. It was time to use automation to contend with the scale of the problem. 

Hash-based signature-based detection works on the basis of 'calculating the hash' for a given file, usually a binary file representing an executable. It uses a well-known hashing algorithm (e.g. MD5, SHA256) to generate a unique, fixed-length string. In turn, this acts as a key identifier for an executable file, regardless of what filename it was given or perhaps what other file attributes were changed. When a file gets scanned by AV software, either as part of a scheduled scan or on-demand when the executable file is being invoked, the hash is calculated and this key is used to check whether the binary executable is known malware. This involves checking it against a database registry of known malware, usually shipped with the AV install and subsequently updated or, in some instances, checked using a remote lookup. 

The challenge here is similar to that of maintaining a distributed database, or perhaps more precisely, that of a content delivery network (CDN). New malware is constantly being discovered, which means the hashes for these files need to be added to the database. Next, this new info needs to be distributed either directly to the endpoint running the AV software or, depending on the design of the solution, mirrored to a server geographically close to endpoints. Calls across a network are subject to the laws of physics as well as network latency and having access to a server - or _Point of Presence (POP)_ - is one way antivirus software performance can be improved. Various strategies for sharing updates and reducing network traffic, such as caching results, are employed to make the task of signature checking efficient. This is a complex problem when one considers the scale of the operation needed to support millions or even hundreds of millions of endpoints. The rate at which new malware is discovered and the rate at which existing malware can modify itself ever so slightly in order to generate a different hash is a huge challenge for the industry, to the point where it's arguably easier to store and manage hashes for known-good software. 

Code Signing using Public/Private key encryption and Trusted Identification using Certificate Authorities (CA) start to be employed as a way to designate software binaries as 'safe' and originating from where they claim. The 'signing' part means that the binary hasn't been tampered with and the certificate validation means that it has come from who it says it has. This provides a quick way for AV vendors to quickly establish OS modules and third-party apps as trusted, without having to go to the expense of calculating hashes, etc. Every vendor wants to provide an efficient solution that doesn't adversely affect the user experience and this is one way that _Realtime Protection_ (RTP), which means checking a binary at time of execution, can efficiently extricate itself early from the protection lifecycle. 

### 2000's

Antivirus software started to employ behaviour-based analysis to detect malware. This consisted of both static and dynamic analysis. The former consisted of statically analyzing the code instructions in the executable to determine if there was malicious routines or unusual references to system files, etc. The latter consisted of run-time interrogation of what the program does after the process started, including what you might describe as _just-in-time_ checking of operations to determine if it was malicious. Of course, this decade saw the explosion of the internet and the additional threats it brought with it so features such as e-mail protection and network protection (e.g. firewalls) began to be incorporated into solutions. Rules-based engines were used to keep up with the rate of malware discovery, by way of providing a faster and more flexible mechanism to deploy detection logic to the endpoint. 

The rate at which new malware samples emerge means that signature-based checks are limited to the extent that updates can be applied and distributed to millions of devices, usually in the form of daily updates. To help with the scalability challenge, protection is extended by also employing heuristic checking along the lines of... _"if it walks like a duck, talks like a duck, well then, it's probably a duck!"_. If an executable looks like malware and acts like malware, then it's probably malware, at least until it is _safelisted_. Advanced Rules-Engines backed by dynamic scripts were introduced to perform these checks. As possible detections are made and as more is learned about both malware and _goodware_, rules are constantly refined and pushed out to the endpoint. This means that substantial infrastructure is required to enable rules analysis, trial and testing and distribution. 


### 2010's
The threat landscape continued to evolve and with that, the sophistication and persistence of threats increased. Big Data and Machine Learning emerged as new technologies in the wider industry and were employed by AV vendors in the backend as a way to perform dynamic behavioural analysis off the target machine. System events related to a programs runtime behaviour were relayed to a data-lake where logic produced by Data Scientists could examine behaviour pattens against large numbers of samples and use statistical analysis to convict 'greyware'. More specific 'Detection & Response' solutions emerged as the industry began to acknowledge that it couldn't achieve 100% protection all the time. In what was a general application of dynamic protection, detection and response used Machine Learning deployed in cloud-based services to examine runtime behaviour and highlight _indicators of compromise_ (IOC) to dedicated Security Operations Analysts. By this time, large organizations and government bodies deployed dedicated Security Operations Centres (SOC) with dedicated analysts to monitor management environments in order to discover and remove malware inside their networks. 

By the mid-2010s, Ransomware had became a thing - a worldwide problem - leading to anti-ransomware (ARW) solutions to detect and block ransomware behaviour as well as measures to roll-back and undo the effects of ransomware. Of course, this decade also saw the rise in Cloud Computing and, as well as utilizing cloud to support Endpoint Protection, protecting assets in the cloud itself became necessary as the threat landscape changed. 


### 2020's
While it remains to be determine how the remainder of this decade pans out, we can see that there has been a greater emphasis on Endpoint Detection and Response as a measure to detect malware after the fact. The range of source information that feeds the backend models continues to grow, earning it the title of XDR. The focus on data science continues and recent inroads in AI have been added to help recognize patterns both on the endpoint itself and in the large models in the backend. Managed Detection and Response has become a service to serve SMEs that typically, don't have the resources to manage their own SOCs. 

The concept of Zero Trust Security has become popular both for devices on the network and for users. The network is assumed to have been compromised and there is a greater emphasis on _Identity and Access Management_ (IAM) to ensure every user on every device is authenticated. Further, the need to protect the user's identity over the user's device has become of practical importance. 

There has been a general shift away from traditional signature-based solutions towards signature-less solutions, employing not only advanced behavioural detection and AI but forms of virtualization (e.g. sandbox detection) and remote detonation earning them the term _Next-Gen AV_ or NGAV. 


### Summary

I started writing this article as a way to describe the depth of antimalware technology at work in protecting devices, explaining how antimalware solutions have evolved. But, as a friend pointed out, it's really a case of co-evolution: how both malware and antimalware has evolved. Of course, malware authors always have an upperhand in this cat and mouse game because they get to move first and they only need to be successful on occasion. Antimalware has to react and be successful each and everytime. 


## Protection Components
The following is a glossary of the various technologies that go into protecting modern day desktops and laptops.

| Component | Description |
| --------- | ----------- |
| Antivirus  | Typically means 'basic' checking of an executable by examining its signature or performing static analysis checks. |
| Advanced Protection  | This usually means a form of advanced antivirus or next-gen antivirus that uses heuristics and other methods other than signature-based mechanisms to detect malware. |
| Realtime Protection  | Performing dynamic analysis and checking executables at runtime, either just as the process is starting or monitoring behavior for a period after starting. |
| Rootkit Scanning  | The ability to detect and prevent attempts by malware to gain elevated privileges on a host.  |
| Anti-ransomware  | This is antimalware software developed to specifically detect and in some cases remediate ransomware. |
| MBR & Disk Sector Protection  | The ability to detect and prevent attempts to write to the _Master Boot Record_ or the Disk Sector of the file system. These are elevated functions that the vast majority of normal software applications do not need to directly access. |
| Network Access Control  | This is similar in effect to Web Protection and protects the user by preventing any software component accessing known bad or malicious IP addresses. Whereas Web Protection works in the context of the browser, Network Access Control mechanisms are typically embedded in AV solutions and work across the entire host.  |



## Internal  Components
There are internal technology components to antivirus solutions that have evolved over time as well. 

| Technology | Description |
| ---------- | ----------- |
| Self Protection  | This describes the ability of an antivirus solution to defend itself from either accidental or deliberate attempts to disable and/or remove it from the host.  |
| Hooking  | This describes the ability of an antivirus solution to embed itself in other running programs and intercept (i.e. 'hook') certain system calls before they get executed. It enables the protection software to examine the type of calls being made and analyze the 'intentions' of the program in real-time to determine whether the operation should be allowed to proceed. For example, certain operating system calls might be blocked when referencing certain resources. In fact, 'Hooking' can be used to effect _Self Protection_ by intercepting function calls to delete the AV executable files. 
| On-access Scanning | This is the technology under the hood that enabled real-time protection where a process is scanned as it is being accessed - i.e. OAS. This is actually quite a technically challenging task because not only does it involve working around the normal operation of the OS starting a process, such that various checks can be performed, but it has often had to achieve this with little or no cooperation from the OS vendor.|



## Additional Technologies 

As well as Endpoint Protection and Endpoint Detection & Response solutions, security on devices can be improved by deploying additional technologies, such as Firewalls, DNS Filtering, E-mail Protection, Data Loss Prevention (DLP). The following is a brief description of the primary technologies deployed, some of which are included in an _Antivirus_ solution as standard.


| Technology | Description |
| ---------- | ----------- |
| Web Protection  | This usually refers to in-browser protection that a) Blocks pop-ups in your browser window, b) Prevents the user for going to known bad or malicious websites and c) either blocks or checks file downloads for viruses. Additionally, some solutions build in measures to deal with unwanted adware. The internet is a primary source of malware so a good Web Protection or Browser Guard solution offers a great deal of endpoint protection by removing malware before it ever gets on the host. |
| Firewall  | Firewalls can be both on the network and on a personal device and act as a filter for network traffic, either blocking or allowing certain IP addresses, blocking or allowing certain ports or blocking or allowing certain protocols on a port. For example, a firewall can restrict communication to HTTP/HTTPS traffic only on port 80, which means internet traffic is allowed but nothing else (e.g. FTP, SNMP). |
| DNS Filtering  | Communication with another host, be it directly or via a Web Browser, will most typically require the resolution of a _Domain Name_. By deploying a secure DNS Filter, users can be kept safe by preventing the resolution of known bad domains. Further, the use of predetermined lists of _allowed_ and _blocked_ sites restricts access and keeps users safe. |
| Gateway  | Most organizations have long-since used Network Gateways to control traffic to and from their organization by routing it through a gateway where access can be controlled. |
| E-mail Protection  | Malware often finds its way on to devices through attachments via email so e-mail protect is an important tool to reduced this threat vector. Depending on its implementation, it may block email or remove suspect attachment, highlight or block SPAM or malicious emails and generally reduce the threat from this avenue. |
| DLP  | While _Data Loss Prevention_ mechanisms are more often employed by organizations to prevent their IP being leaked, actively blocking things like removable USB devices offers a degree of protection by preventing the distribution and activation of malware through this route.  |
| Risk & Compliance  | Compliance tools work by determining which devices in an organization are compliant and managing those that are not. In this context, compliance might mean that certain policies and/or settings are enabled or it might mean that certain OS patches have been applied or that certain OS version upgrades have occurred.  |



***
_Donnacha Forde_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_


## References

[Cybersecurity 101 - Hashing](https://www.sentinelone.com/cybersecurity-101/hashing/) _by SentinelOne_

[Code Signing](https://en.wikipedia.org/wiki/Code_signing) - Wikipedia

[Antivirus Software](https://en.wikipedia.org/wiki/Antivirus_software) - Wikipedia

