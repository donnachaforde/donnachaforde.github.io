[home/](../../../)[blogs+posts/](../../)[software-engineering/](../)[api-design](./)


# API Design - Don't be Overly Considerate


<p align="center">
    <img src="https://c.pxhere.com/photos/6a/38/stones_pebbles_round_stack_zen_stones_zen_stone_background_natural-680859.jpg!d" width="400" height="400" >
</p>

_Photo Credit: [pxhere.com](https://pxhere.com/en/photo/680859)_

### Getting the Balance Right

When crafting a set of APIs, it's crucial to consider the ergonomics of the interface but resist the temptation to overly prioritize convenience. Providing 'convenience' functions may seem helpful, but it can leave your interface susceptible to misuse. Allow me to illustrate this with an example.

Several years ago, when I was working as a Lead Engineer on a systems management project, I was tasked with implementing a feature that would effectively operate the system in read-only mode based on using a database instance imported from a customer's environment. Besides implementing the internal measures necessary to exhibit the read-only behavior, I provided the backend logic needed to manage these databases, such as import, export, activate, list, view and remove a database. Another team member, a Front-end Developer, was responsible for integrating this functionality into the user interface.

Recognizing that a common use-case involved importing a database and immediately activating it, I introduced a helper API `importAndActivate()` that performed both these operations in one call. While this seemed sensible at the time and was straightforward to implement, it ultimately allowed the Front-end Developer to conclude that it was the only call in the entire API set that they needed. In turn, they proceeded to duplicate all the logic relating to managing the databases described above, rendering large parts of my implementation redundant. This proved hugely problematic as the architecture reflected a infrastructure layer that was effectively circumvented by the Front-end Developer's approach and it generated significant technical debt.

This underscores the importance of designing APIs with essential functionality only, avoiding an excess of convenience that might leave it susceptible to abuse. It's crucial to recognize that other developers may not always follow the typical usage pattern, emphasizing the need to design APIs that are minimalist in nature. 

On reflection, up to that point, I'd never quite understood the meaning of the saying _"never give a sucker an even break"_. I guess it means that being overly accommodating can sometimes backfire. Forcing the user of my API to make two calls instead of one would have been justified if it ensured that all the designed functionalities were used appropriately. The key takeaway here is to strike a balance - be helpful, but not excessively so.




#api #design #api-design #interface #lessons-learned 

***
_Donnacha Forde_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_

