---
layout: post
title: "How To Become A Hacker"
description: "What I would have liked to have read when starting out."
date: 2020-04-19
categories: essay
---

#### Why this document?

When I was thirteen and starting high school I read [ESR's][ESR] blog post: [How To Become A Hacker][How To Become A Hacker]. I was excited to learn about the community of programmers working together to build things across the internet and it led me to try installing Fedora Core 4 and eventually Ubuntu 6.06, through which I learned a lot about troubleshooting on my own and trying to get things to actually work. This ended up being critical in developing the skills that helped me get the job I have now. I read about Python and wrote trivial programs, and decided I wanted to study computer science and understand how computers actually work. It was a pretty influential post for me at a time when I wasn't sure what I wanted to do. 

Growing up in the suburbs of Buffalo, NY can be pretty isolating - and while I was lucky that my dad programmed an Apple II in college for fun (so had some background / hacker spirit), he didn't know a lot about more modern software development. I liked computers and played with them, but I didn't know much about what was possible or where to even look to understand more. When the search space is so large and there are a lot of unknown unknowns it can be hard to even find good sources of information to learn from. Being able to select good sources of information requires some existing knowledge and without the guidance of an experienced person it can be difficult. I think things are probably better now that the internet is more developed, but in some ways there are even more options to sift through now than there were then.

Sixteen years later, I thought it'd be fun to write my own version of How to Become a Hacker to supplement ESR's original: something I would have liked to have read myself at thirteen that focuses on some other aspects I would have found helpful too. A lot of posts about programming and related topics are rallying cries, trying to persuade you to adopt a specific programming language, framework, operating system, or specific way of doing things. This post does less of that and while I make some suggestions, it's a more tempered view. Its goal is to fill the niche of what I think I would have liked to have read after ESR's original post (so you should read that one first).

#### There's a lot to learn
In the beginning I remember reading articles and books, but not understanding a lot of the jargon - this is normal. Things that at first seem incomprehensible slowly become understandable as you're exposed to more of it and dig into each thing you don't understand. It's good to just keep reading and powering through, looking things up as you don't understand them and [asking questions][Asking Questions] when you can (ESR also has a good post about [how to ask good questions][How To Ask Questions The Smart Way]). 

Everyone learns something for the first time at some point and things will slowly build until you're more comfortable with the basics. I remember not understanding little details (like not knowing to enter commands in the terminal to run them, or that cd stood for 'change directory'). You pick up on these things from exposure and the more you play with them, the more you're exposed to and the more experience you'll accumulate. If you're lucky enough to live in an area that has a community of people interested in software you'll be able to learn faster.

#### Don't be afraid of things you don't understand
Learning something new that's complicated often feels difficult at first - if it feels easy it may be something you already know or you may not really be testing your knowledge (it's a lot easier to read about how to solve a physics problem and think 'this makes sense' than it is to solve a problem yourself with the tools you just read about). The struggle can be a good sign - it means you're really learning and by focusing on doing similar types of things it'll become easier as you get better. 

I think there's even a bit of advantage that completely new people can have with this: when I develop a little bit of experience it becomes easy and comfortable to just do the thing I already know how to do rather than learn something new. This can lead to getting stuck in a plateau where you just repeatedly do the thing you already know how to do, like a person that can only play one song on guitar and always just plays that one song. To a new person everything is hard so that's not really an option. 

Learning something complicated for the first time should feel a little painful - you should get used to that feeling since it's a good thing and means you're growing. Don't let it scare you away because you don't think you're smart enough. Since there's so much to learn and a lot of different avenues to go down (just in computers there are things like computer graphics, security, machine learning, algorithms, mobile, web, infrastructure, etc.), having a mindset where you allow yourself to grow and get out of your comfort zone to learn new things is critical.

#### Learning to program - learn by doing
Learning to program by just reading a book about programming is like learning to sky-dive by only reading a book about sky-diving. You probably need to read the book (and in the beginning you'll need it as a place to start) - but it won't stick unless you're also writing little programs alongside it. A carpenter gets better by building things, a writer gets better by writing things, and a programmer gets better by programming things. This doesn't mean you shouldn't read books or that good books aren't extremely valuable (they are), but that it's easy to fall into a trap where you read books about programming without actually doing anything yourself because it's easier to read about it then it is to do it, and it can be difficult to come up with something to program in a vacuum when you're starting out.

I agree with ESR that Python is a good language to start with, and there's a nice site online called [learn Python the hard way][Learn Python The Hard Way] that is focused on beginners and uses exercises along the way to teach.

In the beginning the syntax is hard to understand and a lot of time is spent focused on that when you're learning. Since every programming language has different syntax, they seem very different. Then you start to get a handle on syntax and it's more about the general structure of how problems are being solved and what data structures are used. Eventually you get comfortable with common data structures and then the discussion turns to higher levels of abstraction and more general designs or infrastructure that make things easier to manage at scale or easier to change in the future. 

Learning about data structures is the most important next step after getting a handle on a the syntax of a language and being able to write simple programs. There are a few core data structures that are pretty well detailed in the [Cracking the Coding Interview][CTCI] book (along with example problems). Confusingly, languages tend to have different names for their implementation of the same data structures (Python calls hash tables 'dictionaries' for example), but most languages will have some implementation of the core data structures even if they have a unique name.

Troubleshooting or debugging is also a core programming skill - most of programming is actually debugging, so if you like debugging problems this is a probably a good sign. Don't get discouraged when you have to search around a lot to try and understand something or when the documentation you're reading doesn't work, or when you're hitting some unexpected error in your environment - this is normal (it isn't a reflection of your ability). 

Most software doesn't work and there are constantly undocumented errors, bugs, and little details that are hard to get right. For example, most open source projects on Github will have some sort of build system which handles getting the software configured to run. This will do things like pulling in dependencies (other code it requires to work), along with executing any necessary commands to actually get the thing running. If you were to download an interesting project on Github and try to run it you'd probably hit unexpected errors with this process that are often not documented.

Running these errors down and working through problems is normal and something experienced programmers have to deal with too (if we're lucky we've just seen that type of problem before). I've seen people hit errors like this and think they're doing something wrong, but it's not you - it's just how things are. There are a lot of competing tools and even industries around build systems and trying to make them better (which can make things more confusing for beginners since there's no real standardization, and the right way to configure software to run varies depending on programming environment and language).

#### How does a computer actually work?
I remember being frustrated that it was hard to find information about how a computer actually worked. Everything I looked for just talked about computers in unhelpful oversimplified analogies (the disk being the 'filing cabinet for files'), but nothing I could actually read to understand how things *really* worked so if transported into the past I would be able to actually explain how to build one. This is more electrical or computer engineering than software specifically, but there's still a lot of value in understanding the hardware aspects (and it's interesting!).

The best book I'd recommend for this is [Code][Code] by Charles Petzold. It walks you through the basics starting with electrical bits all the way up through the history of Boolean logic and circuit design - with actual drawings of simple circuits and how you can store bits in memory. This builds on itself in the historical context of the discoveries until you've built a small CPU. He also goes into some assembly and basic computer graphics. The author is a really clear writer and teacher so the book is surprisingly readable for the amount of detail.

For more historical context I'd recommend [The Dream Machine][The Dream Machine] by M. Mitchell Waldrop and [Hackers][Hackers] by Steven Levy. Narrative stories make it easier to learn and remember things and I think the context of the discoveries helps in learning how things actually work.

#### Software Tools - Code Editors, Programming Environments
Tools are fun and it's good to know your tools, but you can spend forever customizing things and arguing over little details that don't really matter that much.
Customizing tools can be a fun way to learn when you're starting out, but I've seen people spend enormous amounts of time on this when it generates relatively little value compared to actually writing programs to solve problems or just learning more about the craft of programming in general (a good example book for this currently is [Designing Data Intensive Applications][Designing Data Intensive Applications]). I think focusing on customizing tools too much can hold you back.

Don't worry too much about things like Vim or Emacs or which OS you're using - you can learn the core skills anywhere (this is my biggest disagreement with ESR's post). That said, playing with Linux was a really valuable way for me to learn a lot about troubleshooting - largely because it didn't work very well and I had to spend hours on things like trying to get wireless internet to function, getting the laptop to suspend successfully, even getting the UI to show up at all (things are a little better now). 

I started with trying to install Gentoo (which never actually succeeded). This troubleshooting skill was really instrumental in allowing me to get the job I have now, so if it's fun for you to play with a different OS I'd definitely encourage it, I just don't think it's a requirement. It is probably easier to learn on macOS or Linux though since most of the existing tooling targets those environments and most programmers are using one of those two.

One specific tool worth mentioning is version control, specifically git. It's worth spending some time getting comfortable with the basics, but probably isn't something to focus on until after you've been programming for a bit.

#### Don't research forever
It's easy to procrastinate by 'researching' options forever before starting a project - it can be fun to read about and explore what's available and it's good to do a little of this, but you can also get stuck doing this forever. When in doubt just pick the most popular project that's been around for a while and use that one. If it's popular it probably has a decent community you can learn from and if it's been around for a while it'll probably be more stable (or at least it'll be more substantial and less likely to be abandoned).

#### Computer Science
I really enjoyed studying computer science and think it's still probably the best way to go for the most opportunity (particularly if you live in a suburban area like I did without a lot of software people around). If possible I think it's probably good to try and get into the best CS program you can. There are also a lot of classes available from good programs online, but if your life was like mine was in high school it'll be hard to actually take advantage of this at home.

#### Programming Interviews
If learning is the naive solution to getting good grades, then working on cool programming projects is the naive solution to doing well in programming interviews. To be in a good position for programming interviews at competitive companies you need to get very comfortable with the problems on [leetcode][Leetcode] and the *Cracking the Coding Interview* book. Programming interviews require a lot of practice and are a distinct skill to develop in and of themselves. 

You can go through an entire CS degree and still not know how to program - you can also get a CS degree and still not be able to do programming interviews (both of these are probably the default case). Learning to program and learning to do well in programming interviews takes focused time on your own. CS helps with some direction and focused projects (Lambda School is probably better at this on the programming side and maybe will end up better overall), but you'll have to own a lot of this learning yourself.

#### Roles and Jobs
There are lots of different kinds of roles other than 'software engineer'. There's SRE (Site Reliability Engineer) focused more on infrastructure the code is running on and writing software focused for that. There's internal tools and devops - devs that focus on all of the tooling required to automate how the software is built and tested (read [The Phoenix Project][Phoenix Project] for a fun narrative story illustrating this). There are roles that interact more with users like developer support engineer (helping users with APIs and running down bugs or configuration issues). There are people that focus on game engines, people that focus on VR or computer graphics. There are people that write new computer languages and new compilers. 

In each of these roles there is even more specialization depending on what products are being used and new tools that are being created to solve new problems. Computer security is also an interesting area that I don't know too much about that I think ESR is too dismissive of in his post, but it's also a hard place to start because it requires a lot of existing understanding of how things work to know how things can be broken. I remember picking up this book early on, but I didn't know enough at the time to really understand it: [Hacking: The Art of Exploitation][Hacking The Art of Exploitation]

Of course there's also starting your own company and building your own role that way too as a founder.

#### There's a lot to learn (again)
A lifetime is a long time and a specialization isn't forever, so dive into different things - play with a lot of new things and have fun along the way.

#### Bonus: Community

ESR talks about joining a local Linux users group, but at least for me that was not realistic when I read his post, both because there aren't really that many of them and because I couldn't get anywhere myself that easily since I was too young to drive. There are some online communities that I find interesting now that I think I would have found interesting then too.

**[Hacker News][Hacker News]:** Ycombinator's news site (startup incubator in the bay area). Comments can be hit or miss, but the good ones are really good and a lot of people in the industry hang out there. Paul Graham and Jessica Livingston were the founders of Ycombinator and Paul writes a lot of interesting [essays][PG]. 

**[Twitter][Twitter]:** Largely dependent on who you're following, but can be a great place if you want it to be. It can be hard to know who to follow starting out, but you can look at everyone I follow for a start.

**[Less Wrong][Less Wrong]:** Not programming focused, but there's a decent amount of overlap between the rationality community and the programming community and I like a lot of the writing there, it's definitely something I would have liked to have found around the same time I found How to Become a Hacker. Here's an example post I like a lot: [Disputing Definitions][Disputing Definitions]

#### Contact

I have more articles and books that I liked linked in my [about][About] page.

I remember ESR responding to some email I sent about getting an iPod to work in Fedora Core 4 around the time I read his post and I'm pretty sure Richard Stallman responded to some email I sent around that time too. I thought that was nice. In the spirit of continuing that, feel free to reach out to me with specific questions if you like. 

#### Translations

Thanks to contributions from interested readers, this post has been translated into other languages.

Japanese: [Part 1][Japanese Translation Part 1] [Part 2][Japanese Translation Part 2].

Russian: [Russian Translation]

[How To Become A Hacker]: http://www.catb.org/~esr/faqs/hacker-howto.html
[Learn Python The Hard Way]: https://learnpythonthehardway.org/python3/
[Hacking The Art of Exploitation]: https://www.amazon.com/Hacking-Art-Exploitation-Jon-Erickson/dp/1593271441
[How To Ask Questions The Smart Way]: http://catb.org/~esr/faqs/smart-questions.html
[CTCI]: http://www.crackingthecodinginterview.com/
[Code]: https://www.amazon.com/Code-Language-Computer-Hardware-Software/dp/0735611319
[The Dream Machine]: https://www.amazon.com/Dream-Machine-M-Mitchell-Waldrop/dp/1732265119/
[Hackers]: https://www.amazon.com/Hackers-Computer-Revolution-Steven-Levy/dp/1449388396
[Leetcode]: https://leetcode.com/
[Designing Data Intensive Applications]: https://dataintensive.net/
[About]: https://zalberico.com/about/
[Hacker News]: https://news.ycombinator.com/
[Twitter]: https://twitter.com/zachalberico/following
[Less Wrong]: https://www.lesswrong.com/
[Disputing Definitions]: https://www.lesswrong.com/posts/7X2j8HAkWdmMoS8PE/disputing-definitions
[Phoenix Project]: https://www.amazon.com/dp/1942788290
[PG]: http://paulgraham.com/articles.html
[ESR]: https://en.wikipedia.org/wiki/Eric_S._Raymond
[Asking Questions]: https://zalberico.com/essay/2017/02/21/asking-questions.html
[Japanese Translation Part 1]: https://itnews.org/news_contents/how-to-become-a-hacker-1
[Japanese Translation Part 2]: https://itnews.org/news_contents/how-to-become-a-hacker-2
[Russian Translation]: https://howtorecover.me/kak-stat-hakerom
