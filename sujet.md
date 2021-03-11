# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

**The year 2038 bug** [News from *The Guardian*](https://www.theguardian.com/technology/2014/dec/17/is-the-year-2038-problem-the-new-y2k-bug)
The 32-bits based systems will experience a remake of the Y2K bug from the 19<sup>th</sup> January 2038 at 03:14:07. This bug is global and is due to the limited representation of integers in 32-bits based systems. Indeed, the datetimes encoding will overflow the maximum value that can be stored in a 32-bits integer by that date. The maximum value is 2<sup>31</sup>-1 seconds after the 1<sup>st</sup> January 1970, thus the following second will be computed as a negative value, which means the datetime will be interpreted as the 13<sup>th</sup> December 1901 at 20:45:52. This bug may cause several embedded systems (based on 32-bits processor architectures) to dysfunction and to be totally unusable. As examples, it can be said smartphones, some cars using datetime, internet routers and databases using MySQL will experience severe issues because of this bug. The bug was found by AOL in their AOLserver software in May 2006 when they fixed a timeout delay of 1 billion seconds for a "never time out" request. And 1 billion seconds is equivalent to 32 years, in 2006, 32 years forward means after 2038 so after the bug's trigger limit.<br/>
Complementary source: [Wikipedia](https://en.wikipedia.org/wiki/Year_2038_problem)

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-769?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

The following bug is present in a test. When testing MultiValuedMap.
To check the content of the map we use the toString function of MultiValuedMap. Only the order in which toString displays the elements of the map is arbitrary, while the test compares with a hard string where the order of the elements is already imposed.
To solve the bug the tests in question have been modified. It is a question of checking element by element that they are present in the map. The comparison with a hard string is still used, only the arbitrary order is not a problem anymore.

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

Netflix isn't the only company performing those experiments, Amazon, Google, Microsoft and Facebook are knwon to use Chaos Engineering as well in order to test their services.<br/>
The engineer pays close attention to the "**steady-state behavior of the system**".<br/>
A chaos engineering experiment revolves around 4 principles according to Netflix's criteria:

1. <ins>Build a hypothesis around steady-state behavior</ins><br/>What matters the most at Netflix is the **availability** of the services if one shuts down, all the others should still work. A metrics that describes a great steady-state behavior of the system for Netflix is the **SPS** (stream) starts per second which shows how many users at a given second start watching a video. In addition, the number of registrations per second is also a good metrics. If there is no streaming or registration during a well-known peak, it clearly shows an unavailability of those services for the users.

2. <ins>Vary real-world events</ins><br/>Sampling from all the inputs that may happen in reality. Picking up past failure inputs to verify the bugs are fixed and inputs are working fine by now.

3. <ins>Run experiments in production</ins><br/>The whole Netflix software can't be tested before production because it is too huge it is launched as a distributed software, thus it communicates through the Internet which varies every second.

4. <ins>Automate experiments to run continuously</ins><br/>For a constant changing system, it is necessary to automate the experiments so the system can be tested systematically.

* The *Chaos Monkey* strategy is to shut down some virtual machines hosting their services randomly. They need to work only during work hours so the coworkers can react quickly in case there is a failure. *Chaos Monkey* runs continuously during working days.
* The *Chaos Kong* strategy consists of simulating failures of a part of the cloud services of Amazon covering an entire region ( [Amazon EC2 region](https://docs.aws.amazon.com/fr_fr/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions) ). *Chaos Kong* is run monthly.
* *Failure Injection Testing* ( *FIT* ) forces the Netflix services to fail to communicate. The Traffic and Chaos team pays close attention to the system which is attended to "degrade gracefully".
* When shutting down a whole region, the system is supposed to fallback over a healthy cloud region. The team speculates that the transition should have no impact over the SPS.
* Inject latency into requests between services instead of cutting the bridge between 2 services.
* Fail an internal service and see if it can disrupt the global behaior of the system.

To sum up over Chaos Engineering at Netflix:

1. Define metrics to measure the steady-state behavior of the system describing a normal behavior.

2. Assume the steady-state is initially respected in both the control group and experimental group.

3. Input real world events

4. Try to find abnormalities in the metrics between the control and experimental groups.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

A formal specification of webassembly ensures important properties of the language. For example, the security, speed, portability and compactnessof the language. But although a formal specification can guarantee some important properties, it is only a specification. The implementation of the specification may have errors or the specification may also be wrong. It is important to check every property of the language.  

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

