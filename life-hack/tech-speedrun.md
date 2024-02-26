## [How to get into software](https://github.com/npmaile/blog/blob/main/posts/2.%20How%20to%20get%20into%20software.md)

1. Install [Arch Linux](https://archlinux.org/) on your main computer and use it for ALL computer tasks.
2. Read [Automate the Boring Stuff](https://automatetheboringstuff.com/)
3. Master a text editor.
4. Create a profile on [Github](https://github.com/) and learn the basics of version control with git.
5. Learn [Markdown](https://www.markdownguide.org/)
6. Start lurking on [Hacker News](https://news.ycombinator.com/) and [Lobsters](https://lobste.rs/)
7. Write a website from scratch, and host it on a server somewhere.
8. Read [The Go Book](https://www.gopl.io/)
9. Research functional programming, and write a program
10. [Leetcode](https://leetcode.com/) (just do around 20)

## [Teach Yourself Programming in Ten Years](http://www.norvig.com/21-days.html)

1. Get interested in programming, and do some because it is fun. Make sure that it keeps being enough fun so that you will be willing to put in your ten years/10,000 hours.
2. Program. The best kind of learning is learning by doing. To put it more technically, "the maximal level of performance for individuals in a given domain is not attained automatically as a function of extended experience, but the level of performance can be increased even by highly experienced individuals as a result of deliberate efforts to improve." (p. 366) and "the most effective learning requires a well-defined task with an appropriate difficulty level for the particular individual, informative feedback, and opportunities for repetition and corrections of errors." (p. 20-21) The book Cognition in Practice: Mind, Mathematics, and Culture in Everyday Life is an interesting reference for this viewpoint.
3. Talk with other programmers; read other programs. This is more important than any book or training course.
4. If you want, put in four years at a college (or more at a graduate school). This will give you access to some jobs that require credentials, and it will give you a deeper understanding of the field, but if you don't enjoy school, you can (with some dedication) get similar experience on your own or on the job. In any case, book learning alone won't be enough. "Computer science education cannot make anybody an expert programmer any more than studying brushes and pigment can make somebody an expert painter" says Eric Raymond, author of The New Hacker's Dictionary. One of the best programmers I ever hired had only a High School degree; he's produced a lot of great software, has his own news group, and made enough in stock options to buy his own nightclub.
5. Work on projects with other programmers. Be the best programmer on some projects; be the worst on some others. When you're the best, you get to test your abilities to lead a project, and to inspire others with your vision. When you're the worst, you learn what the masters do, and you learn what they don't like to do (because they make you do it for them).
6. Work on projects after other programmers. Understand a program written by someone else. See what it takes to understand and fix it when the original programmers are not around. Think about how to design your programs to make it easier for those who will maintain them after you.
7. Learn at least a half dozen programming languages. Include one language that emphasizes class abstractions (like Java or C++), one that emphasizes functional abstraction (like Lisp or ML or Haskell), one that supports syntactic abstraction (like Lisp), one that supports declarative specifications (like Prolog or C++ templates), and one that emphasizes parallelism (like Clojure or Go).
8. Remember that there is a "computer" in "computer science". Know how long it takes your computer to execute an instruction, fetch a word from memory (with and without a cache miss), read consecutive words from disk, and seek to a new location on disk. (Answers here.)
9. Get involved in a language standardization effort. It could be the ANSI C++ committee, or it could be deciding if your local coding style will have 2 or 4 space indentation levels. Either way, you learn about what other people like in a language, how deeply they feel so, and perhaps even a little about why they feel so.
10. Have the good sense to get off the language standardization effort as quickly as possible.

## [[97 Things Every Programmer Should Know - [Henney].pdf |97 Things Every Programmer Should Know]]

1. Better do it right than to do it quick. Technical debts is not your friend
2. Use virtual mentors. Find authors and developers on the Web who you really like and read everything they write. Subscribe to their blogs
3. Get to know the frameworks and libraries you use. Knowing how something works makes you know how to use it better. If they’re open source, you’re really in luck. Use the debugger to step through the code to see what’s going on under the hood. You’ll get to see code written and reviewed by some really smart people.
4. Many incremental change is better than one massive change
5. Personal preferences and ego should not get in the way. Thinking you can do a better job than the previous programmer is not a valid reason either
6. Beware the share. Check your context. Only then, proceed
7. Always leave the campground cleaner than you found it
8. The purpose of code reviews should be to share knowledge and establish common coding guidelines. Sharing your code with other programmers enables collective code ownership
9. Exception is an alternative return path that is part of the model and that the client should be aware and be prepared to handle
10. A developer even a senior developer should never have access beyond the development server
11. Provide data and context (stack trace, logs, exception, etc...) when asking help from another technical person
12. Create a small program to test partial logic of a bigger program. Unit test is similar but once test suite grows large enough, compiling a small program is way faster
13. Make an interface easy to use correctly, and hard to use incorrectly. Interfaces exist for their users not their implementer
14. Control user interactions. Provide instructions or cues to guide user journey
15. Re-inventing the wheel helps to teach intimate/deeper knowledge of an implementation/system
16. SRP is for independent deployment
17. Try to get acceptance criteria from all stakeholders (PO and QA) before implementing a new feature