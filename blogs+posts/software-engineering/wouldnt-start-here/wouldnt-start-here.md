[home/](https://donnachaforde.github.io)[blogs+posts/](https://donnachaforde.github.io/blogs+posts/)[software-engineering/](https://donnachaforde.github.io/blogs+posts/software-engineering/)[wouldn't-start-here](./wouldnt-start-here.md)

# Well, if I were you, I wouldn’t start from here…




<img src="https://www.nicepng.com/png/detail/201-2016290_windows-mac-linux-logo.png" alt="Windows Mac Linux Logo@nicepng.com" >

_Photo Credit: [www.nicepng.com](https://www.nicepng.com/maxp/u2w7w7y3e6t4w7t4/)_


### Abstract
_The fragmentation of the target platforms for AV vendors due to the increased diversity of desktop/laptop OS and underlying processor architecture in recent years means that the engineering mechanisms and organizational structures that have evolved over decades may not serve us as well into the future. We must consider the underlying architecture design of our endpoint solutions to determine whether they are fit for purpose for the coming decades. Breaking down product siloes and drawing a distinction between those parts of our solutions that are entirely platform specific and those that are generic is perhaps the first step towards adopting a more modular approach and even a ‘microservices architecture on the endpoint’, which may well lead to more effective and more efficient software development._



So goes the joke about giving directions. And, perhaps the same can be said of Endpoint Architecture in cyber security because the solutions we typically have today are as a result of evolution, not necessarily planning.  Let’s examine how that evolution has affected our solutions today…

### Malware has Disproportionately Targeted Windows
Consequently, it’s a pretty safe observation that endpoint protection software is weighted towards the Windows platform, specifically Windows OS on Intel Architecture (i.e. Wintel). Endpoint solutions for Windows typically have the richest feature-sets – and – the research teams underpinning those features have the greatest depth of expertise on that platform. This should come as no surprise because endpoint-protection solutions are ‘demand-led’. They’re a direct result of revealing and understanding the tactics and techniques employed by malware-authors as they play out a _cat-and-mouse game_. And, malware-authors have long-since targeted Windows primarily because that has historically been the platform with the greatest number of deployments – and malware is, to a large extent, a numbers game. 

> **Note:** _Even today, the majority of malware targets Windows x86 (32-bit)._



### Protection Parity is a Misconception
There has been much discussion, led by customer demand, about achieving closer ‘parity’ in endpoint protection across desktop OS. In practice, this usually means parity between Windows and macOS because Linux on the desktop is comparatively niche. Apple have made significant inroads into the enterprise desktop and laptop space over the last decade and as a consequence, customers seek a uniform solution that provides protection in equal measure across Windows and macOS. This is a grey area because, as mentioned above, certain protection features are tightly-coupled to the underlying OS and Windows and macOS are fundamentally different operating systems. So, protection mechanisms built for Windows often don’t necessarily map to macOS because the underlying constructs in the OS are different. 
On the other hand, it’d have to be acknowledged that certain features available today on Windows are not available on macOS for other reasons, which have more to do with staffing and resources. Given the extent of the knowledgebase within an organization on Windows, it’s understandable when we see an innovative feature emerge for Windows but then don’t see the equivalent for macOS. That, in part, relates to the availability of talent and the extent to which the internal knowledgebase has developed. 

### Organizational Structure & Resource Allocation Reflect Market-share 
Of course, market-share is a factor too that gets reflected in the allocation of resources with Windows still dominating with ~70-80% of the desktop/laptop market and macOS having somewhere between 7.5-20% (depending in which stats you look at – see references below). If you project relative revenue earnings and in turn, engineering funding by this sort of distribution, then you can begin to understand why it becomes a challenge to achieve ‘partity’. The smaller macOS teams cannot achieve the same delivery velocity.

### Organizational Evolution
Another factor that affects organisational structure is the evolution of engineering organizations over time. Unlike products originally intended to sell across platforms from the get-go, security solutions have historically tended to start with Windows, then get replicated on macOS or other platforms. I say ‘replicated’ because the evolution of those organizational structures practically prohibits porting code across platforms. For example, if an engineering team is targeting the Windows platform, then even if they’re using a programming language that lends itself to portability (e.g. C/C++) then unless there is an overarching design philosophy, it’s quite difficult to develop code and libraries that don’t become ‘polluted’ with OS specific constructs. By and large, compiler vendors are the same OS vendors and they seem to do their best to lock you in to developing for their platform alone. You have to work pretty hard and take certain measures to ensure your C/C++ code, their dependencies and your build system are cross-platform compatible. Given where many AV companies have come from, it’s practically impossible to retrospectively adapt their solutions stacks to be portable. I should clarify that the context here relates to all the code that ‘could’ be common across platforms. For the most part, this is everything other than the ‘business logic’ of protection itself because, as we’ve discussed, that’s more often tied to the underlying OS. So, we’re talking about pretty much everything else, which might be categorized as supporting services (e.g. telemetry, logging, licensing, policy enforcement, version control, content updates, etc.). 
The upshot is it seems that AV vendors, perhaps more than other software houses, end up with discrete engineering teams supporting Windows, macOS and Linux, without any shared components. Here, Conway’s Law makes its presence felt and the challenge of achieving protection parity clearer. 
 
### Market Fragmentation
At one time, an AV vendor could afford to target a Windows only solution and still be assured that they had a significant market presence. Today, however, macOS has made a significant inroad into the enterprise desktop market. Linux and even ChromeOS are further eroding this dominance. The upshot is that if an AV vendor wants to meet their customers’ needs and provide a single-vendor solution, then the AV vendor has to split their talent resource-pool across these heterogeneous platforms and development environments/paradigms. 
The expected market adoption of arm64 processors across Windows and macOS further exacerbates this challenge. There are technical differences across the platforms that determine how an engineering organization responds to supporting these platforms but, at least for Windows, full native support involves a significant investment across engineering, quality assurance and build & release that is unlikely to see a return until customer demand sufficiently increases. 
The upshot is that in order to sell your product today, you have to deal with a more fragmented target platform, be that a different processor architecture or different OS. The overarching question then must be whether endpoint architecture has a part to play in influencing the design such that code production, maintenance and field support can be easier. Today, we pretty much entirely have a ‘shared nothing’ model due to the way this space has evolved. As discussed, considerable parts of any AV solution have to be ‘shared nothing’ due to the nature of the problem domain and target platform but does that mean we cannot have a ‘shared something’ model?

### Summary
The challenge today for AV vendors is that the ground has shifted under our feet. Where once a Windows only solution may have been sufficient for the majority of customers, the steady rise in the adoption of MacBooks coupled with the more niche deployment of Linux variants plus the rapid adoption of Chromebooks, particularly in educational settings, means that providing a holistic solution to customers requires greater effort all round, across product management, engineering, threat-research, quality-assurance, etc. Understandably, customers want to engage a single vendor for their entire deployed base, be it a mix of Windows, macOS, ChromeOS and Linux. The upshot is from the protection-provider’s perspective is that the entire organization has to be geared up to bring products and solutions across these platforms. Where once there was a single target platform – and as a consequence, a single testbed, a single build & release platform, there are now multiple. 


### References
https://arstechnica.com/gadgets/2021/02/the-worlds-second-most-popular-desktop-operating-system-isnt-macos-anymore/
https://gs.statcounter.com/os-market-share/desktop/worldwide/





***
_Donnacha Forde_

_August 2003_

_[LinkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_

