---
title: Recycling, reuse, and codebase heuristics
slug: recycling-reuse-code-heuristics
date: 2015-07-03 12:00:00+00:00
tags: ci quality qa code
category:
link:
description:
type: text
---

Did you know that people tend to recycle _unused_ sheets of paper, but tend to throw used paper in the bin? Or that people are more likely to throw away a crushed drinks can, while they'll recycle a pristine one? It turns out that we make very weird decisions about what to re-use.

> During an experiment, marketing professor Remi Trudel noticed a pattern in what his volunteers were recycling versus throwing in the garbage. He then went      through his colleagues' trash and recycling bins at Boston University for more data.
> 
> He found the same pattern, says NPR's Shankar Vedantam: "Whole sheets of paper typically went in the recycling, but paper fragments went in the trash."
> 
> Same type of paper, different shapes, different bins.
> 
> Trudel and fellow researcher Jennifer Argo conducted experiments to figure out why that might be. Volunteers received full pieces of paper as well as fragments, and they also received cans of soda.
> 
> "After the volunteers had drunk the soda, when the cans were intact, the cans went in the recycling," Vedantam tells Morning Edition host David Greene. "But if the cans were dented or crushed in any way, the volunteers ended up putting those crushed cans in the trash."

It's a really interesting phenomenon. You can read more about it in the <a href="https://www.npr.org/2013/09/27/226580668/how-recycling-bias-affects-what-you-toss-where">original NPR piece</a>, or listen below: 

<iframe src="https://www.npr.org/player/embed/226580668/226716486" width="100%" height="290" frameborder="0" scrolling="no"></iframe>
What does this have to do with anything?

It's no secret that the most successful agencies are those which can commit to delivering quality software without crumbling under deadline pressure. We typically have ten to twenty simultaneous projects in our studio, and there are huge levels of potential technical debt, all of the time. We constantly need to make decisions about when to invest time into our core software and infrastructure, and a large part of recognising that investment is in re-use of our existing code.

Since our very early projects at FARM, we have followed a model of _build, adapt, extract_; where we first build bespoke software, adapt it to fit more than one project, then extract it to it's own repository for future re-use. And, like all software companies, we struggle with this. A given code module might be just too specific to the needs of project for which it was written, or adapting it to serve multiple projects would stretch the code to breaking point. It's a difficult problem to address, but a very important one.
  
In this environment, where the _adapt_ and _extract_ stages are often initiated by our developers independently of any 'big picture' strategy, it's interesting to consider what drives those decisions. Often we look at code which solves 90% of the problem at hand, yet we choose to rewrite, rather than re-use. Why is this?

I think there are some interesting unconscious decisions people make. In the same way people make snap decisions about what to recycle, I think most programmers develop a mental heuristic for what constitutes 'good' code, and quite often those decisions don't appear to be rational. There are a few things which I look for:

- _Recent activity_. Most of the code which we need to evaluate on the fly is open-source, or is available internally via Bitbucket. The very first thing I look at is 'freshness'.
- _Documentation_. A sure sign of a poorly maintained module is that is has no documentation, or a generic `README` which doesn't actually describe the project. 
- _Convention_. For external packages, I look for good Python packaging practices. I want to install software which is a good citizen, and won't screw up my project namespace. For internal projects, I look for evidence of a proper virtualenv, frozen `requirements.txt`, and sensible naming conventions. 
- _Concepts_. We generally deal with Django projects, and looking at the models which are defined in a module is a good indicator of the high-level concepts. If these overlap enough with the problem domain, the re-use is probably going to be beneficial. 

A lot of these factors are very easy to assess. In the same way an experienced developer can 'eyeball' a source file, with practice you can 'eyeball' at this level of abstraction, too. The first three items above are easy to develop a gut feeling for (less so the final one), having read enough code. Just like the blank sheet of paper is more likely to be recycled, the package that looks pristine is more likely to be reused.        
            