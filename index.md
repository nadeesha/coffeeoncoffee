# Why we need to move

Jaggery uses Rhino. There's nothing inherently wrong with that. But Rhino was written for a JVM which was not as mature, performant and as fast as the JVM today. There are many things that could make Rhino fast, but Rhino can not adopt. That was the reasoning behin Nashorn, Oracle's replacement for Rhino. Compared to Rhino, Nashorn is faster, lighter and an undisputed successor to Rhino.

We really can not count on Jaggery adoption by the masses unless we force it upon them, or unless we change how we do things.

# Why we can't move to Node

It's simple. We have a bunch of stuff that have been tried and tested in real world problems, that run in the JVM. If we move to Javascript, it doesn't make sense to lose what we have done in the process. Node, does not allow us to use our jars.

# Our competitiors

In a post-Rhino world, there are really two competitors to running Javascript on the JVM. Nodyn, and Avatarjs. We are going to evaluate these two objectively in order to get and idea on how we should move ahead with our own framework - Jaggery.

# Under the hood

## Nodyn 
Nodyn runs on dyn.js javascript runtime, Compared to avatar.js, which uses Oracle's Nashorn. A truly objective comparison between these are to be done. But out of the box, the major difference is that dyn.js is open-sourced with an apache2 license. As a OS advocates, we have a real chance here to go ahead and contribute to the runtime iteself, without being mere consumers of it.

It also uses vert.x which introduces itself as "... a lightweight, high performance application platform for the JVM that's designed for modern mobile, web, and enterprise applications". Nodyn has clustering support built in with the addition of Vert.x. Vert.x gives some nice advantages to Nodyn, and also forces another dependency on someone using Nodyn for the long run.

## Avatarjs
Avatar.js embraces the new Nashorn runtime engine by Oracle. Although Nashorn is closed source, Avatar is not. It does everything Nodyn does, except for the OOTB perks that vert.x gives, and does it with a cleaner architecture with less moving parts. It's only dependency except for the JVM, is Nashorn.

Although there's no other runtime which surpasses V8, Nashorn is much much faster than it's predecessor Rhino, which Jaggery uses now.

# Community

Nodyn has a growing community, but it's not considerably large. To put things in perspective, it only has 11 contributors on Github. Avatarjs is being actively developed by people inside Oracle, some of whom lead the Nashorn initiative as well.

# Coding Style

Jaggery was founded on the belief that we should make it easier for Java developers to write Javascript. One of biggest problems with JS is the callback hell. In java, you can do something like this:

```java
doSomething();
doAnotherThing();
doThisFinalThing();
```

You can be guaranteed that these three functions will execute in order. But if you do this in javasctipt, (i.e. V8)

```js
doSomething(function() {
	doAnotherThing(function () {
		doThisFinalThing();
	});
});
```

This is because JS works on an evented model and is single threaded. It doesn't block on I/O. In any instance where your function does an I/O call, it defers that operation, and continues with the program execution. In java, this is equivalent to a thread being spawned on every I/O call. With threads, you get race conditions, all over and you need to handle that. Because of that, we in java choose our battles with threads. Whereas in javascript, (although we don't work with threads in the same sense) you can not subscibe to the synchronous coding style when your program does I/O. 

This make Javascript really fast, suitable to work with front-ends (browsers do not freeze on a form submit because of this reason), but makes it a tad bit harder to write.