# API Design - Don't be overly considerate

Try not to be overly considerate to your end users of your API design, lest you find your interface abused. Let me explain by way of an example. 

I was tasked with implementing a feature to our backend management system that would allow the user, or in my case, the Front-end developer, to load a database and have the system run off of this persistence store rather than the live system. It was essentially a tool to run the system in read-only mode and was used to examine customer's systems. 

These databases, which were single files consisting of a proprietary store format, were meant to be exported, imported and activated. In addition, because several could be imported, the list of databases needed to be viewed and items could also be removed. 

In summary, that meant the API to the feature had to support the following operations:

* import
* export
* activate
* deactivate
* list
* remove

The typical usage pattern was that a user would _import_ a database and then _activate_ it so as to browse its contents, etc. Given this typical pattern, it seemed natural to provide a 'convenience' interface that simply combined these steps. So, I added:

* importAndActive

Now, I'd have to admit some long-standing influence here. Years earlier, having reviewed a more experienced 'C' programmers code, I was impressed with how they'd handled errors and quit the program with an aptly named `flagAndExit()` function that both flagged the error to `stdout` and exited the program with the relevant exit-code. It was the use of the 'conjuction' that stuck. In my case `importAndActive()` made perfect sense and reflected the typical use-case. 

What could possibly go wrong? 

When I was learning to drive and having mastered the mechanics, my father tried to impart some wisdom on being safer on the road by trying to anticipate other drivers' behavior. He used to say "People are liable to do ANYTHING..." and how right he was. There isn't an experienced driver out there that hasn't seen some bizarre, unexpected and unexplainable behavior on the road. 

It must be the human condition. For when I learned how well-designed, thought-out API was used, I just couldn't believe it. The only API that ever got invoked was `importAndActivate()`. None of the other APIs were ever used, which meant the vast majority of the code used to store and manage the database instances was never exercised. Instead, the Front-end developer had built their own solution for storing and managing databases leaving the absolute only API they needed being, you guessed it, `importAndActivate()`. 

I could never have guessed that my API would've been circumvented in this way. But, the facts of the matter were that it was only made possible because I tried to be helpful. I've struggled to understand that phrase _"never give a sucker an even break"_ but I think it could be applicable in this case. I simply should not have so helpful. So what if they have to make two API calls instead of one? The use-case was still implemented as a single operation - it didn't really warrant the extra API call. 

The moral of the story... Sure, be helpful, but not overly helpful. 
