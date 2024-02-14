[home/](../../../)[blogs+posts/](../../)[software-engineering/](../)[endpoint-protection](./)


# A Glossary of Endpoint Protection Technologies

<img src="https://c.pxhere.com/photos/5f/b9/computer_gaming_green_keyboard_microsoft_pc_technology_typing-916984.jpg!d" srcset="https://c.pxhere.com/photos/5f/b9/computer_gaming_green_keyboard_microsoft_pc_technology_typing-916984.jpg!d" width="1000" height="400">

_Photo Credit: [pxhere.com](https://pxhere.com/en/photo/916984)_


### Introduction
Most people don't realize how many discrete technologies go into modern-day endpoint protection solutions. The term _Antivirus_ or AV has become overloaded and arguably, doesn't quite describe the array of protection measures deployed to your device. 

Within cybersecurity, for the most part AV really means traditional antivirus protection, which scans, detects, quarantines and removes known malware from your host. There is often a remediation aspect to AV, which involves 'cleaning' or undoing the effects of the known malware. Remediation is another overloaded term so in this instance, this form of remediation is often referred to as _basic remediation_. 

In all, modern Endpoint Protection (EP) includes a collection of technologies added over time to address the emerging threats of the day. For instance, Ransomware really only became something of interest to threat researchers circa 2015 but hit news headlines repeatedly during 2016 as various versions of Ransomware caused havoc around the globe (e.g. WannaCry, Petya, NotPetya). Cybersecurity vendors reacted and sought to add anti-ransomware protection and anti-ransomware remediation. 

The point is that modern solutions are a collection of technologies and while there are some that regard Endpoint Protection as a commodity, the fact is the difference between an _Efficacy_ Rate of 94% and 97-98% is the difference between being protected and perhaps, not really being protected at all and being vulnerable to _Zero Day Threats_. 

### A Short History of Antimalware

Arguably, the Antimalware industry was born pretty much right after the idea of self-reproducing software (or virus) came to light. Early incarnations were more of a nuisance than they were malicious and the sector didn't really get going until the 1990's. Early investigations into infections of rootkit, boot sectors and executable files led to scan and clean solutions. Viruses were most likely spread by sharing infected floppy disks in the days before the internet. 

* **Pre-1990's** - The concept of a virus emerged and there was a rise in malicious viruses targeting IBM PCs. 

* **1990's** - This decade saw a significant increase in the number and complexity of malware and the emergence of companies like McAfee and Norton. Initially, the focus was on signature-based detection but by the end of the decade, the focus had shifted to a more advanced-heuristic methods in order to detect malware. 

* **2000's** - Antivirus software started to employ behaviour-based analysis to detect malware. This consisted of both static and dynamic analysis. The former consisted of statically analyzing the code instructions in the executable to determine if there was malicious routines or unusual references to system files, etc. The latter consisted of run-time interrogation of what the program did after the process started, including what you might describe as _just-in-time_ checking of operations to determine if it was malicious. Of course, this decade saw the explosion of the internet and the additional threats it brought with it so features such as e-mail protection and network protection (e.g. firewalls) began to be incorporated into solutions. 

* **2010's** - The threat landscape continued to evolve and with that, the sophistication and persistence of threats increased. Ransomware became a thing, leading to anti-ransomware (ARW) solutions to detect and block ransomware behaviour as well as measures to roll-back and undo the effects of ransomware. 


Antivirus solutions at the time consisted of checking these items for known signatures and patterns. 

Defining 'patterns' either in code pages or in actions, such as the presence of new files, etc. became the focus of the industry for a while. Not every malware signature was known and some were polymorphic meaning that each time they replicated, they created a slightly different version of their binary, which threw Virus Scanners off the scent. AV solutions worked with a form of database that contained signatures of known malware plus 'rules' providing instructions on how to detect malware from patterns. If we couldn't rely on detecting malware by its signature, then we'd have to detect it by applying rules of things to check that would indicate that something was malware. These were both static checks and dynamic checks, meaning that the executable was being examined at runtime. 


### Protection Components
The following are descriptions of the various technologies that go into protecting modern day desktops and laptops.

 Technology    | Description |
| -------- | ------- |
| Antivirus  | Typically means 'basic' checking of an executable by examining it's signature or performing static analysis checks. |
| Realtime Protection  | Checking executables at runtime, either just as the process is starting or monitoring behavior for a period after starting. |
| Advanced Protection  | This usually means a form of advanced Antivirus Typically means 'basic' checking of an executable by examining it's signature or performing static analysis checks. |



***
_Donnacha Forde_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_




