# Key principles of software development 

There are few principles everyone who writes software should follow. 

Main idea is to make it easier to program, maintain and use code. Almost everyone, especially 
roboticists tend to forget importance of clean code and simplicity, which in turn results 
in all sorts of problems such as unreadable code, unmaintainable code and finally, which 
is worst of all, **unusable code**. I would be brave enough and call those principles, 
**a basic engineering principles** beacuse there's no possibility to develop 
any kind of complex system without following them. 

I'm often saddened when I see really great and smart stuff in banged up code. A lot of 
code you write, will be used only by yourself which makes you think that it's not 
neccessary to follow those principles, however, **your future self will be forever greatful 
to you if you simply learn to follow them all along.**

A lot of times I thought I will be faster if I just write something and don't care 
about anything aside pure functionality, and that's fine for some rapid prototyping, 
however, if you plan to work on something for more than week or two, I really 
suggest to folow those principles. You'll probably be slower in the short term, 
however, in the long term you'll be significantly faster. 

## KISS (Keep it Simple, Stupid) 

Simple systems work best when they're kept simple. Avoiding uneccessary complexity 
will make your system more robust, easier to understand, easier to reason about, and easier to extend. 

```
Remember that whenever you add a new dependency to your project, or start using that fancy new framework, or create a new micro-service, you’re introducing additional complexity to your system. 
You need to think whether that complexity is worth it or not.
```

You can watch following [talk](https://www.callicoder.com/software-development-principles/) on that topic which 
simply says that you shouldn't walk away from complexity, you should **run away from it** 


## DRY (Don't repeat yourself) 

Don't write same code/configuration in multiple places. If you do that, than you'll have to maintain them and keep them in 
sync, which means more work for you. 

DRY principle promotes reusability, which is quite cool idea. Create small software blocks and use them to build 
house. 

## YAGNI (You aren't gonna need it) 

Avoid adding functionality to your program if you're not needing it now. 

You probably won't be needing it in the future, and it will wind up as some stale part of the 
code that makes it harder to understand or use. 

Implement stuff only when you're 100% sure you need it. 

You can write some note about idea at the moment, however keep your head in the main task, and 
don't implement it until you're sure you need it, or you want to try it. 

Remember following quote: 

```
Premature optimization is the root of all evil
```

## SOLID principles 

Guidelines to follow when building software so that it is easier to scale and maintain. 

They're also quite important, however, for programming newcomers, mentioning them and writing about them would be 
adding uneccessary complexity to this post. :)

If you want to know more about them, 
you can find some useful information on following: 
 * [wikipedia](https://en.wikipedia.org/wiki/SOLID) --> short and brief
 * [digitalocean](https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design) --> has examples  
 * [comic](https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898) --> cool ilustrations 
