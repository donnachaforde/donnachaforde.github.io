[home/](https://donnachaforde.github.io)[blogs+posts/](https://donnachaforde.github.io/blogs+posts/)[code-design](https://donnachaforde.github.io/blogs+posts/code-design/)



# Design Considerations on the espresso library

### Introduction

I've learned that our 'thought process' can be improved by simply _putting pen to paper_ and writing out such things as facts, assumptions, outstanding unknowns, etc. The act of writing or typing out these details seems to act like an assistant to problem solving in a similar way that peer code-review helps the author clarify their logic. 

This lesson became really clear to me at one point in my career when I inherited responsibly for supporting and maintaining a product in the field that had large numbers of cases opened against it. I was the last line of support so the _buck stopped with me_ and typically, by that stage a case could have a long case history, which made it difficult to extract the facts from conjecture. In order to cope with this sort of challenge, I adopted the habit of summarizing the case histories detailing known facts, assumptions and the theories as to the cause. Not only did it provide assurance to all stakeholders, but I found the process to be invaluable at getting to the crux of the issue and I regularly surprised myself in how I was able to arrive at a root-cause by the time I'd finishing my analysis. 

Nowadays, I adopt much the same process when designing code solutions. 

>**Tip -** Writing things down helps the thought process.

Furthermore, when working with languages like C++ and Java (but particularly with C++) there are often so many choices on how to achieve a solution that the mind can easily become boggled. The act of writing down what you what to achieve, why you want to do it - and listing influences on your thought-process - really helps clarify those choices, It also explains the _design journey_ that the code, representing the end-stage, can't quite ever reveal. 


### Design Ruminations

* [Design Considerations](./espresso/design-considerations)
* [Creation Pattens](./espresso/object-construction)
* [Employing Manager Objects](./espresso/manager-objects)





