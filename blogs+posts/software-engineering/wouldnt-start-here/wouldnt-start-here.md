[home/](https://donnachaforde.github.io)[blogs+posts/](https://donnachaforde.github.io/blogs+posts/)[software-engineering/](https://donnachaforde.github.io/blogs+posts/software-engineering/)[wouldn't-start-here](./wouldnt-start-here.md)





<img src="https://www.nicepng.com/png/detail/201-2016290_windows-mac-linux-logo.png" alt="Windows Mac Linux Logo@nicepng.com" >

_Photo Credit: [www.nicepng.com](https://www.nicepng.com/maxp/u2w7w7y3e6t4w7t4/)_


### _Abstract_
_The fragmentation of the target platforms for AV vendors due to the increased diversity of desktop/laptop OS and underlying processor architecture in recent years means that the engineering mechanisms and organizational structures that have evolved over decades may not serve us as well into the future. We must consider the underlying architecture design of our endpoint solutions to determine whether they are fit for purpose for the coming years. Breaking down product siloes and drawing a distinction between those parts of our solutions that are entirely platform specific and those that are generic is perhaps the first step towards adopting a more modular approach and ultimately, even a ‘microservices architecture on the endpoint’, which may well facilitate more effective and more efficient software development of endpoint security solutions._

# Well, if I were you, I wouldn’t start from here…

So goes the joke about giving directions. And, perhaps the same can be said of Endpoint Architecture in cyber security because the solutions we typically have today are as a result of evolution, not necessarily planning. If you were tasked with designing an architecture to support what's needed today, chances are you very likely wouldn't start from here...

There are two factors generating friction against each other:

1. Historically, AV solutions are more heavily weighted towards Windows than any other OS.
2. The target market for Endpoint has fragmented.

Let's examine these factors in a bit more detail. 

## Wintel Weighting

There are serveral reasons why anti-malware solutions are typically  disproportionally more sophisticated on Windows, specifically on Intel processor architecture. 

1. Historically, Windows has domainated the desktop/laptop space and there's simply a larger target base.
2. there have been more Windows machines over a long period. Antimalware solutions for Windows are disproportionately more sophisticated than on any other OS.
2. Expertise and Organizational Structures 
2. Protection parity across heterogeneous OS is a misconception. 




 Let’s examine how that evolution has affected our solutions today…



### Malware has Disproportionately Targeted Windows

Malware authors have most frequently targeted Windows and, consequently, it’s a safe observation that endpoint protection software has historically been weighted towards Windows, specifically Windows on Intel Architecture (i.e. Wintel). Endpoint solutions for Windows typically have the richest feature set. Further, the research teams underpinning those features have the greatest depth of expertise on Windows. This should come as no surprise because endpoint-protection solutions are primarily ‘demand-led', They’re often a direct result of uncovering and understanding the tactics and techniques employed by malware-authors as they play out a _cat-and-mouse game_ as they try to obscure their malicious intensions. 

And, malware-authors have long-since targeted Windows primarily because that has historically been the platform with the greatest number of deployments. To a large extent, Malware is a numbers game. 

By and large, the AV community has more experience addressing Windows malware: there's simply more of it, there has been for the longest time - and consequently, there's a better trained and experienced muscle-response to these threats.

> **Note:** _Even today, the majority of malware is Windows x86 binaries (i.e. 32-bit)._



### Protection Parity is a Misconception - Resource Allocation Reflect Market-share
There's customer-led demand to achieve closer parity in endpoint protection across desktop OS. In practice, this usually means parity between Windows and macOS because Linux on the desktop is comparatively niche. Apple's inroads into the enterprise desktop and laptop space over the last decade or so and has rendered customers seeking a more uniform solution that provides protection in some equal measure across Windows and macOS. This is a grey area because certain protection features are tightly-coupled to the underlying OS and Windows and macOS are fundamentally different operating systems. So, protection mechanisms built for Windows often don’t necessarily map to macOS because the underlying constructs in the OS are different. 

On the other hand, it’d have to be acknowledged that certain features available today on Windows are not available on macOS for other reasons, which have more to do with the dynamics of staffing and resources. Given the extent of the knowledgebase within an organization on Windows, it’s understandable when we see an innovative feature emerge for Windows but then don’t immediately see the equivalent for macOS or even Linux. That, in part, relates to the availability of talent, the depth of experience internal to the organization and therefore, the extent to which it can innovate on other endpoint platforms.  

Of course, market-share is a factor too that gets reflected in the allocation of resources with Windows still dominating with ~70-80% of the desktop/laptop market and macOS having somewhere between 7.5-20% (depending in which stats you look at – see references below). If you project relative revenue earnings and in turn, engineering funding by this sort of distribution, then you can begin to understand why it becomes a challenge to achieve ‘partity’. The smaller, less-resourced teams cannot achieve the same delivery velocity.


### Organizational Evolution
Another factor that affects organisational structure is the evolution of engineering organizations over time. Unlike products originally intended to sell across platforms from the get-go, security solutions have historically tended to start with Windows, then get replicated on macOS or other platforms. I say ‘replicated’ because the evolution of those organizational structures practically prohibits porting code across platforms. 

For example, if an engineering team starts a project intially targeting the Windows platform then, even if they’re using a programming language that lends itself to portability (e.g. C/C++), unless there is an overarching design philosophy to produce portable code, it’s quite difficult to develop code and libraries that don’t just become ‘polluted’ with OS specific constructs. By and large, compiler vendors are the same entity as the OS vendors and they seem to do their best to lock you in to developing for their platform. You have to work pretty hard and take certain measures to ensure your C/C++ code, their dependencies and your build system are cross-platform compatible. You may have to pass up on using platform specific features. 

Given where many AV companies have come from, it’s practically impossible to retrospectively adapt their solutions stacks to be portable. I should clarify that the context here relates to all the code that ‘could’ be common across platforms. For the most part, this is everything other than the ‘business logic’ of protection itself because, as we’ve discussed, that’s more often tied to the underlying OS. So, we’re talking about pretty much everything else, which might be categorized as supporting services (e.g. telemetry, logging, licensing, policy enforcement, version control, content updates, etc.). 

The upshot is it seems that AV vendors, perhaps more than other software houses, end up with discrete engineering teams supporting Windows, macOS and Linux, without any shared components. Here, Conway’s Law makes its presence felt and the challenge of achieving protection parity clearer. 


## Market Fragmentation
At one time, an AV vendor could afford to target a Windows-only solution and be assured that they had a significant market presence. Today, however, macOS has made a significant inroad into the enterprise desktop market. Linux and even ChromeOS are further eroding this dominance. The upshot is that if an AV vendor wants to meet their customers’ needs and provide a single-vendor solution, then they have to spread their resources and talent-pool across a greater number of heterogeneous platforms and development environments/paradigms. 

The expected market adoption of arm64 processors across Windows and macOS further exacerbates this challenge. There are technical differences across the platforms that determine how an engineering organization handles these platforms but, at least for Windows, full native support involves a significant investment across engineering, quality assurance and build & release that is unlikely to see a return for perhaps a number of years until such time that customer demand for that hardware sufficiently increases. 

The upshot is that in order to sell your product today, you have to deal with a more fragmented target platform, be that a different processor architecture or different OS. 



## Summary
The challenge today for AV vendors is that the ground has shifted under our feet. Where once a Windows only solution may have been sufficient for the majority of customers, the steady rise in the adoption of MacBooks coupled with the more niche deployment of Linux variants plus the rapid adoption of Chromebooks, particularly in educational settings, means that providing a holistic solution to customers requires greater effort all round, across product management, engineering, threat-research, quality-assurance, etc. Understandably, customers want to engage a single vendor for their entire deployed base, be it a mix of Windows, macOS, ChromeOS and Linux, not to mention mobile. 

The upshot from the protection-provider’s perspective is that the entire organization has to be geared up to bring products and solutions to market across these platforms. Where once there was a single target platform – and as a consequence, a single testbed, a single build & release platform, there are now multiple. 



The overarching question then must be whether endpoint architecture  has a part to play in influencing the design such that code production, maintenance and field support can be made easier and more cost-effective. Today, we pretty much entirely have a ‘shared nothing’ model due to the way the field has evolved. As discussed, considerable parts of any AV solution have to be ‘shared nothing’ due to the nature of the problem domain and target platform but does that mean we cannot have a ‘shared something’ model?


### References
_ARS Technica_ - [The word's second most popular desktop operating system isn't macOS anymore.](https://arstechnica.com/gadgets/2021/02/the-worlds-second-most-popular-desktop-operating-system-isnt-macos-anymore/)

_Global Stats statcounter_ - [Desktop Operating System Market Share Worldwide](https://gs.statcounter.com/os-market-share/desktop/worldwide/)





***
_Donnacha Forde_

_August 2003_

_[LinkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_

