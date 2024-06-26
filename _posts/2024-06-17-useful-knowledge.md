---
layout: post
title: "Knowledge Worth Learning"
tags: essay
---

What makes someone motivated to learn new things? In my opinion, trying to answer this question is crucial to success in a knowledge economy. Most people have a learning style which is unique to them. For example, I consider myself a highly pragmatic person. I like to remind myself of this from time to time because it helps me to ground my thinking and better understand my motivations and drives. I see learning new things as something that is highly *practical*, akin to an investment in myself that leads to a positive return in the future.

For example, if I'm working on a programming problem, I enjoy learning about an algorithm or design pattern that helps me solve that problem better. Other times, I enjoy listening to podcasts about science-backed techniques for improving personal health (I'm looking at you, Andrew Huberman). I therefore get the greatest enjoyment from knowledge that is *useful* and *actionable*.

Although I like to think this pragmatic approach to learning has served me well, it also has its limitations. In particular, it biases my interest towards knowledge that seems useful *at first glance*. This can be sub-optimal, because the usefulness of knowledge is not always immediately obvious. The question then becomes, *how to identify knowledge that is useful?* 


## What makes knowledge useful?

The idea of useful knowledge reminds me of an amusing scene from [Sherlock Holmes](https://en.wikipedia.org/wiki/A_Study_in_Scarlet). In this scene, Dr. Watson is surprised to learn that Holmes does not know that the earth goes around the sun, as Holmes finds such knowledge irrelevant:

> His ignorance was as remarkable as his knowledge. Of contemporary literature, philosophy and politics he appeared to know next to nothing. Upon my quoting Thomas Carlyle, he inquired in the naïvest way who he might be and what he had done. My surprise reached a climax, however, when I found incidentally that he was ignorant of the Copernican Theory and of the composition of the Solar System. That any civilized human being in this nineteenth century should not be aware that the earth travelled round the sun appeared to me to be such an extraordinary fact that I could hardly realize it. […] He said that he would acquire no knowledge which did not bear upon his object. Therefore all the knowledge which he possessed was such as would be useful to him.

I think Holmes's approach to knowledge raises an intriguing question: *How can one determine upfront whether some piece of information will turn out to be useful?* In the case of trivia about the Solar System, I think you can be pretty sure that this will not be directly useful for daily life (unless, of course, it's your job to know). However, in many other cases, the distinction is much more subtle.

Here's an example. When I was studying computer science at university, I had to implement an algorithm for [sorting an array of numbers](https://en.wikipedia.org/wiki/Sorting_algorithm). The thing to understand is that you would *never* implement a sorting algorithm yourself for any real-world use case. Whatever your choice of programming language is, you can be pretty sure that it already includes an efficient implementation of a sorting algorithm. In other words, writing a sorting algorithm is mostly a theoretical exercise. At the time, it therefore made zero sense to me why I would have to learn and write an implementation of a sorting algorithm. Why not just use the existing implementations, which are clearly better, and not worry about it? I simply could not imagine the practical use of such an exercise.

Later on, I realized the value: Even though you might never write another sorting algorithm again in your life, there is a good chance *you will learn useful things in the process*. Unfortunately, the value of such knowledge is often not immediately obvious. If I had focused only on the immediately useful, I would have missed out on learning knowledge that eventually proved useful for future projects.

To illustrate this point, here are some useful insights I gained from list sorting:

- [Binary search](https://en.wikipedia.org/wiki/Binary_search), a valuable technique for efficiently searching an array of values (and derived from list sorting), and something I have used for various software projects. 
- Being able to recognize sorting problems helps to solve some problems more effectively. Sorting is pretty common in many real-world applications, and recognizing a sorting problem can help you choose the right solution for specific situations. For example, some sorting algorithms, such as [insertion sort](https://en.wikipedia.org/wiki/Insertion_sort), are a better choice when a list is nearly sorted.
- Learning about sorting introduces you to related topics such as big-O notation, recursion, and algorithm memory usage. These concepts are ubiquitous in computer science and will likely be useful in other contexts.
- Job interviews: Knowledge of list sorting and time/space [complexity](https://en.wikipedia.org/wiki/Computational_complexity) is something that is sometimes tested in job interviews for software engineering roles.

To summarize, learning about sorting algorithms may not be immediately useful, but it has a high [ROI](https://en.wikipedia.org/wiki/Return_on_investment) in the medium to long term. Moreover, this type of knowledge lays a foundation that makes future learning easier and more efficient. This is what I want to discuss next.

## The value of foundational knowledge

As you can see, my goal of acquiring useful knowledge is not as straightforward as I might have hoped. Another complication is that unlike university, the real world is messy, and the most useful skills for succeeding in it arguably share little overlap with the skills needed to succeed in university. So, how does one determine which knowledge is worth learning?

Let me first state the obvious by saying that this is highly dependent on your goals. My focus here is on [knowledge workers](https://en.wikipedia.org/wiki/Knowledge_worker) such as software engineers, who often benefit from increasing their knowledge and skills over time. Such a person might also switch domains every now and then, forcing him or her to learn new things regularly.

As a general principle, I would argue there is great value in learning things that are *not too problem-specific*.

This is the difference between *shallow* and *deep* (or foundational) knowledge. Shallow knowledge will help you only in very specific situations, whereas foundational knowledge is less problem-specific but carries over to many other situations, giving you a broader conceptual framework. Elon Musk [has referred to this distinction](https://www.inc.com/jessica-stillman/heres-elon-musks-secret-for-learning-anything-fast.html) in terms of a knowledge tree:

> It is important to view knowledge as sort of a semantic tree -- make sure you understand the fundamental principles, i.e. the trunk and big branches, before you get into the leaves/details or there is nothing for them to hang on to.

An example of the distinction between relatively shallow and deeper knowledge is understanding the specific syntax of a programming language versus learning the underlying principles of how computers operate. The former gives you a basic capacity to do useful work, whereas the latter provides knowledge that is independent of any programming language and can be applied regardless of the language you use. For example, let's say you're writing a computer program that adds numbers to an array. If you understand what happens in the computer's memory when you append an element to an array, you can write a better program by recognizing that a linked list or hash map is more efficient than an array in certain situations. This will make you a better programmer, no matter what language you use.

Seeking out useful knowledge *can be a trap*, because it can focus your attention on the shallow knowledge that is immediately useful in the short term, while diverting your attention from the foundational knowledge you may need to learn in order to reach a deeper understanding of a topic. In other words, your knowledge tree might be missing the trunk and branches on which the leaves hang. Solid foundational knowledge is useful because it allows you to apply [first principles thinking](https://www.youtube.com/watch?v=NV3sBlRgzTI), enabling you to analyze underlying patterns better, which can lead to new insights. [^1]

The problem is that *this is hard*. First, it can be difficult to identify what foundational knowledge you're missing. By definition, foundational knowledge is far removed from specific problem instances, meaning you need to have enough insight to realize which foundational principles are worth studying. Second, it takes more effort to learn foundational knowledge than to learn shallow knowledge. The difficulty lies in deciding [what to learn](https://danluu.com/learn-what/) and how to allocate your time effectively. A lack of understanding of underlying principles and foundational algorithms may lead you to re-invent the wheel and waste a lot of time doing so. Moreover, because the emphasis in modern society is often on short-term thinking, taking the time to learn foundational knowledge can be a [great advantage](https://www.goodreads.com/book/show/25744928-deep-work). 

I think this is worth repeating: *Most people don't do this*. The reason is straightforward: It is hard and takes a lot of time and effort. Instead, it is often tempting to look for *hacks*: health hacks, productivity hacks, etc., where the focus is on quick and maximally useful solutions. The problem with this approach is that it foregoes the opportunity to learn more fundamental principles.

Take nutrition as another example. Instead of following the latest diet fad, you are probably better off studying what exactly makes a diet "healthy" in the first place. If you start from the foundational principles that constitute a healthy diet--such as a certain amount of protein, vitamins, minerals, calories, etc.--you have a great deal of flexibility in designing a diet that works best for your personal circumstances. This is tremendously *empowering*, but also takes a good deal of effort.

On the other hand, partly because it's so demanding, learning foundational knowledge is not always necessary. Sometimes, a quick hack is all you need. For example, taking a vitamin D supplement in winter is a quick and easy way to address the fact that many people are vitamin D deficient in winter due to a lack of sunlight exposure. You don't need to know much more about it than that to reap the benefits.

My impression is that the need for foundational knowledge increases as you try to solve harder and more novel problems, whether in your professional or personal life. This is because if a problem has been solved many times before, there is a good chance someone has documented the solution and shared it with others. If not, it usually pays off to go a few levels deeper than the specifics of a problem might seem to require, to gain a new perspective on the problem. Examples of this include: doing novel research, building a startup or product that addresses a difficult problem, or designing a nutrition plan that works well for you.

Frankly, I still find it quite difficult to determine what information is worth learning. Paradoxically, to know how useful a piece of information is, [you might have to learn it first](https://en.wikipedia.org/wiki/Four_stages_of_competence). Additionally, what is useful to one person [may not be to someone else](https://danluu.com/learn-what/), so copying the same trajectory as others is also not an ideal strategy.

I find inspiration in the people I look up to when I consider their approach to learning. For example, my impression is that the most impressive software engineers tend to share a learning mindset. They strive to understand systems *deeply* rather than superficially. A learning mindset might just be the most important meta-strategy: Make it a habit to expand your knowledge and skills, striving for deep, non-superficial understanding. As long as you keep learning, you'll learn useful knowledge along the way.

Have you ever found yourself learning something that seemed useless at first but turned out to be valuable later? I'd love to hear your experiences in the comments below.

[^1]: One of my favorite examples of this is Andrej Karpathy [making the connection between LLMs and operating systems](https://x.com/karpathy/status/1707437820045062561) -- two things which, at first glance, have little to do with each other.


## Related reading

- [Dan Luu on what to learn](https://danluu.com/learn-what/)
- [Peter Drucker - Managing Oneself](https://web.archive.org/web/20211019055306/https://www.csub.edu/~ecarter2/CSUB.MKTG%20490%20F10/DRUCKER%20HBR%20Managing%20Oneself.pdf)
    > Success in the knowledge economy comes to those who know themselves—their strengths, their values, and how they best perform
- [Andrej Karpathy on how to become an expert](https://x.com/karpathy/status/1325154823856033793):
	> How to become expert at thing:  
	> 1 iteratively take on concrete projects and accomplish them depth wise, learning “on demand” (ie don’t learn bottom up breadth wise)  
	> 2 teach/summarize everything you learn in your own words  
	> 3 only compare yourself to younger you, never to others
- [Andrej Karpathy on the shortification of learning](https://x.com/karpathy/status/1756380066580455557):
    > There are a lot of videos on YouTube/TikTok etc. that give the appearance of education, but if you look closely they are really just entertainment. This is very convenient for everyone involved : the people watching enjoy thinking they are learning (but actually they are just having fun). The people creating this content also enjoy it because fun has a much larger audience, fame and revenue. But as far as learning goes, this is a trap. This content is an epsilon away from watching the Bachelorette. It's like snacking on those "Garden Veggie Straws", which feel like you're eating healthy vegetables until you look at the ingredients.  
    >
    > Learning is not supposed to be fun. It doesn't have to be actively not fun either, but the primary feeling should be that of effort. It should look a lot less like that "10 minute full body" workout from your local digital media creator and a lot more like a serious session at the gym. You want the mental equivalent of sweating. It's not that the quickie doesn't do anything, it's just that it is wildly suboptimal if you actually care to learn.  
    > ...  
    > So for those who actually want to learn. Unless you are trying to learn something narrow and specific, close those tabs with quick blog posts. Close those tabs of "Learn XYZ in 10 minutes". Consider the opportunity cost of snacking and seek the meal - the textbooks, docs, papers, manuals, longform. Allocate a 4 hour window. Don't just read, take notes, re-read, re-phrase, process, manipulate, learn. 
- [Gian Segato: Learning takes effort, otherwise it's just entertainment](https://giansegato.com/essays/edutainment-is-not-learning)
- [Ben Kuhn on exploring new things](https://www.benkuhn.net/exploration/)

---
<br>

