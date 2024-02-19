[home/](../../../)[blogs+posts/](../../)[software-engineering/](../)[endpoint-protection](./)


# A Short History of Antimalware

<img src="https://c.pxhere.com/photos/5f/b9/computer_gaming_green_keyboard_microsoft_pc_technology_typing-916984.jpg!d" srcset="https://c.pxhere.com/photos/5f/b9/computer_gaming_green_keyboard_microsoft_pc_technology_typing-916984.jpg!d" width="1000" height="400">

_Photo Credit: [pxhere.com](https://pxhere.com/en/photo/916984)_


### Introduction
Most people don't realize how many discrete technologies go into modern-day endpoint protection solutions. The term _Antivirus_ or AV has become overloaded and arguably, doesn't quite describe the array of protection measures deployed to your device. More often within cybersecurity circles, Antivirus really just means traditional, signature-based antivirus protection, which scans, detects, quarantines and removes known malware from your host. There is often a remediation aspect to AV, which involves 'cleaning' or undoing the effects of the known malware. Remediation is another overloaded term so in this instance, this form of remediation is often referred to as _basic remediation_. 

In all, modern Endpoint Protection (EP) includes a collection of technologies added over time to address the emerging threats of the day. For instance, Ransomware really only became something of interest to threat researchers circa 2015 but hit news headlines repeatedly during 2016 as various versions of Ransomware (e.g. WannaCry, Petya, NotPetya) created havoc around the globe . Cybersecurity vendors reacted by adding anti-ransomware protection and anti-ransomware remediation features to their products.

The point is that modern solutions are a collection of technologies developed over time as the threat landscape has evolved. While there are some that regard Endpoint Protection as a commodity, the fact is smalls differences in _Efficacy Rate_ (a measure of the extent to which antimalware works), can mean the difference in being protected or being vulnerable to, for example,  _Zero Day_ threats. 

## The Evolution of Antimalware 

In a sense, the Antimalware industry was born pretty much right after the idea of self-reproducing software (or virus) came to light. In this context, the term virus merely meant that a piece of software could replicate and distribute copies of itself, not that it was necessarily malicious. Early incarnations were more of a nuisance than they were malicious and the sector didn't really get going until the 1990's. Early investigations into infections of rootkit, boot sectors and executable files led to scan and clean solutions. In the days before the internet became commonplace in the mid-90's, viruses were more likely spread by sharing infected floppy disks. 

### Pre-1990's
 The concept of a virus emerged and, towards the end of the '80s, there was a rise in malicious viruses targeting IBM PCs. Early scan solutions to detect known viruses and remove them were developed, though these were specifically written to target known viruses. 

### 1990's
This decade saw a significant increase in the number and complexity of malware and the emergence of companies like McAfee and Norton as the cyber security market to protect computers emerged. Initially, the focus was on signature-based detection but by the end of the decade, it had shifted to a more advanced-heuristic methods in order to detect malware at scale. 

Signature-based detection works off the basis of 'calculating the hash' for a given file, usually a binary file representing an executable. It uses a well-known hashing algorithm (e.g. MD5, SHA256) to generate a unique, fixed-length string. In turn, this acts as a key identifier for an executable file, regardless of what filename it was given or perhaps what other file attributes were changed. When a file gets scanned by AV software, either as part of a scheduled scan or on-demand when the executable file is being invoked, the hash is calculated and this key is used to check whether the binary executable is known malware. This involves checking it against a database registry of known malware, usually shipped with the AV install and subsequently updated or in some instances, performing a remote lookup. 

The challenge here is similar to that of a distributed database or more precisely, a content delivery network. New malware is constantly being discovered, which means the hashes for these files need to be added to the malware database. Next, that needs to be distributed either directly to the endpoint running the AV software or, depending on the design of the solution, mirrored to a server geographically close to endpoints. Calls across a network are subject to the laws of physics as well as network latency and having access to a server - or _Point of Presence (POP)_ is one way antivirus software performance can be improved. Various strategies for sharing updates and reducing network traffic, such as caching results, are employed to make the task of signature checking efficient. This is a complex problem when one considers the scale of the operation needed to support millions or even hundreds of millions of endpoints. The rate at which new malware is discovered and the rate at which existing malware can modify itself ever so slightly in order to generate a different hash is a huge challenge to the industry, to the point where it's arguably easier to store and manage hashes for known-good software. 

Code Signing using Public/Private key encryption and Trusted Identification using Certificate Authorities (CA) starts to be employed as a way to designate software binaries as 'safe' and originating from where they claim. This provides a quick way for AV vendors to quickly establish OS modules and third-party apps as trusted, without having to go to the expense of calculating hashes, etc. 

### 2000's

Antivirus software started to employ behaviour-based analysis to detect malware. This consisted of both static and dynamic analysis. The former consisted of statically analyzing the code instructions in the executable to determine if there was malicious routines or unusual references to system files, etc. The latter consisted of run-time interrogation of what the program did after the process started, including what you might describe as _just-in-time_ checking of operations to determine if it was malicious. Of course, this decade saw the explosion of the internet and the additional threats it brought with it so features such as e-mail protection and network protection (e.g. firewalls) began to be incorporated into solutions. Rules-based engines were used to keep up with the rate of malware discovery, by way of providing a faster and more flexible mechanism to deploy detection logic to the endpoint. 

The rate at which new malware samples emerge means that signature-based checks are limited to the extent that updates can be applied and distributed to billions of devices, usually in the form of daily updates. To help with the scalability challenge, protection is extended by also employing heuristic checking along the lines of... _"if it walks like a duck, talks like a duck... then, it's probably a duck."_. If an executable looks like malware and acts like malware, then it's probably malware, at least until it be _safelisted_. Advanced Rules-Engines backed by dynamic scripts were introduced to perform these checks. As possible detections are made and as more is learned about both malware and goodware, rules are constantly refined and pushed out to the endpoint. This means that substantial infrastructure is required to enable rules analysis, trial and testing and distribution. 

### 2010's
The threat landscape continued to evolve and with that, the sophistication and persistence of threats increased. Ransomware became a thing, leading to anti-ransomware (ARW) solutions to detect and block ransomware behaviour as well as measures to roll-back and undo the effects of ransomware. 




Antivirus solutions at the time consisted of checking these items for known signatures and patterns. 

Defining 'patterns' either in code pages or in actions, such as the presence of new files, etc. became the focus of the industry for a while. Not every malware signature was known and some were polymorphic meaning that each time they replicated, they created a slightly different version of their binary, which threw Virus Scanners off the scent. AV solutions worked with a form of database that contained signatures of known malware plus 'rules' providing instructions on how to detect malware from patterns. If we couldn't rely on detecting malware by its signature, then we'd have to detect it by applying rules of things to check that would indicate that something was malware. These were both static checks and dynamic checks, meaning that the executable was being examined at runtime. 


## Protection Components
The following are descriptions of the various technologies that go into protecting modern day desktops and laptops.

 Technology    | Description |
| -------- | ------- |
| Antivirus  | Typically means 'basic' checking of an executable by examining it's signature or performing static analysis checks. |
| Realtime Protection  | Performing dynamic analysis and checking executables at runtime, either just as the process is starting or monitoring behavior for a period after starting. |
| Advanced Protection  | This usually means a form of advanced Antivirus Typically means 'basic' checking of an executable by examining it's signature or performing static analysis checks. |
| Anti-ransomware  |  |
| WeAnti-ransomware  |  |



***
_Donnacha Forde_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_


## References

[Cybersecurity 101 - Hashing](https://www.sentinelone.com/cybersecurity-101/hashing/) _by SentinelOne_