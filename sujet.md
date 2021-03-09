# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

Eve Online's bug
Eve Online's is a space-based MMORPG game published by CCP Games in May 2003. On the 5th of December 2007, they released a patch "Eve Online: Trinity" which caused some big troubles in many computers and made them unable to boot anymore. How an online game broke the boot of windows ? In an article, Dr. Erlendur S (software engineer at Eve's online) wrote in an article

> we started receiving reports that [...] content upgrade was causing problems to players by deleting the file C:\boot.ini, which is a Windows system startup file. In some cases the computer was not able to recover on the next startup and would not start until the file had been fixed.

The bug comes from a mistake made by engineers relative to absolute paths. In the patch, there is a script that deletes a file named "boot.ini" (which is located in the installation directory of the game). Instead of deleting the "boot.ini" file using `Delete "$INSTDIR\boot.ini"`, the engineers wrote `Delete "boot.ini"` without the $\INSTDIR before. The assumption was that by setting `SetOutPath "$INSTDIR"` at the beginning of the script will change the working directory for all the following commands.

The engineer also wrote
> Why doesn't Windows protect its system startup files? That's a good question, one that I have asked myself in these last few days and wish I knew the answer. But of course I'm not going to blame Microsoft for our mistake. Windows doesn't protect those files and therefore software developers must take care not to touch them. We should have been more careful.

In my opinion both windows and Eve Online's have a responsability in this issue. Such a mistake should have beet anticipated by Microsoft and they should have think about protect the boot.ini file or create a copy of it.

> **Why wasn't this caught in a code review?** The installer scripting language is not easy to read. We had been working very hard for many weeks prior to release, evenings and weekends, and this error slipped by us.
[...]
**Why didn't you catch this during testing?** It's partly the reason above, not enough time to test the graphics content upgrade thoroughly to notice it removed this file.

Maybe a tool to find which file on the disk are wrote/deleted by the installation process would have highlighted the bug.

source : https://www.eveonline.com/article/about-the-boot.ini-issue

---

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-769?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

The following answer is about this issue : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-709?filter=doneissues

The bug seems to be local as it is relative to a mistake in a method implementation and can be highlighted with a simple unit test.

The bug is that removing the last element of their MultiSet implementation doesn't set the collection size to 0.

Note : What is a multiset : 
> There are two main ways of looking at Multiset :
This is like an ArrayList<E> without an ordering constraint i.e, ordering does not matter.
This is like a Map<E, Integer>, with elements and counts.
The total number of occurrences of an element in a multiset is called the count of that element.

The reason is a simple miss in the remove method, when removing the last element of an item (when number to remove >= element actual count) the count of the item should be set to 0 which was not done.

In the PR, the author also added a Junit test which reproduces the scenario of the bug so it can be tracked in the future.

https://github.com/apache/commons-collections/pull/66/commits/8e2842abdd1584c4d23c62a11c9fc2277cf4d124

---

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

