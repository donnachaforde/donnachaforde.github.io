[home/](https://donnachaforde.github.io)[blogs+posts/](https://donnachaforde.github.io/blogs+posts/)[software-engineering/](https://donnachaforde.github.io/blogs+posts/software-engineering/)[api-design](./api-design)

# API Design - Don't be Overly Considerate


<p align="center">
    <img src="https://c.pxhere.com/photos/6a/38/stones_pebbles_round_stack_zen_stones_zen_stone_background_natural-680859.jpg!d" width="400" height="400" >
</p>

_Photo Credit: [pxhere.com](https://pxhere.com/en/photo/680859)_

### Getting the Balance Right

When crafting APIs, it's crucial to consider the ergonomics of the interface but resist the temptation to overly prioritize convenience. Providing 'convenience' functions may seem helpful, but it can leave your interface susceptible to misuse. Allow me to illustrate this with an example.

Several years back when I was working as a Lead Engineer, I was tasked with implementing a feature in our management system, enabling it to effectively operate in read-only mode using a database instance imported from a customer's environment. 
Besides implementing the necessary internal read-only behavior, I needed to provide operations to support the front-end (being developed by another team), such as import, export, activate, list, view and remove a database. 

Recognizing that a common use-case involved importing a database followed immediately by activating it, I introduced a helper API called `importAndActivate()`. While seemingly sensible and pretty straightforward to implement, this ultimately led the Front-end Developer to believe that `importAndActivate()` was the only touch-point needed and they proceeded to create pretty much a duplicate implementation. My API was only invoked when the end-user activated a database, rendering a large part of the solution unused.

This underscores the importance of designing APIs with essential functionality only, avoiding an excess of convenience that might leave it susceptible to abuse. It's crucial to recognize that other developers may not always follow the typical usage pattern, emphasizing the need to design APIs that are minimalist in nature. 

The lesson learned here is encapsulated in the phrase, "never give a sucker an even break". My API was circumvented because of an attempt to be overly helpful. Sometimes, requiring users to make two API calls instead of one is justified if it ensures the comprehensive use of the designed functionalities. The moral of the story is to find the right balance - be helpful, but not overly so.



>**Aside:**  Up until then, I never fully understood the meaning of that saying ""never give a sucker an even break."".



#api #design #interface #lessonslearned 

***
_Donnacha Forde_

_[linkedIn.com/in/donnachaforde](https://www.linkedin.com/in/donnachaforde)_




