## Content
[1 Project Summary](#1-project-summary)<br>
[2 Study Design](#2-study-design)<br>
&ensp;&ensp;[2.1 Research Questions](#21-research-questions)<br>
&ensp;&ensp;[2.2 Our Dataset](#22-our-dataset)<br>
[3 Results](#3-results)<br>
&ensp;&ensp;[3.1 Experiment on Dialog Disentanglement](#31-experiment-on-dialog-disentanglement)<br>
&ensp;&ensp;[3.2 RQ1: Communication Profile](#32-rq1-communication-profile)<br>
&ensp;&ensp;[3.3 RQ2: Community Structure](#33-rq2-community-structure)<br>
&ensp;&ensp;[3.4 RQ3: Discussion Topic](#34-rq3-discussion-topic)<br>
&ensp;&ensp;[3.5 RQ4: Interactive Pattern](#35-rq4-interactive-pattern)<br>
[4 Conclusion](#4-conclusion)<br>
[5 Download](#5-download)<br>
[6 References](#6-references)<br>

## 1 Project Summary
&ensp;&ensp;Modern communication platforms such as Gitter and Slack play an increasingly critical role in supporting software teamwork, especially in open source development.
Conversations on such platforms often contain intensive, valuable information that may be used for better understanding OSS developer communication and collaboration. 
However, little work has been done in this regard. 
To bridge the gap, this paper reports a first comprehensive empirical study on developers' live chat, investigating when they interact, what community structures look like, which topics are discussed, and how they interact. We manually analyze 749 dialogs in the first phase, followed by an automated analysis of over 173K dialogs in the second phase. We find that developers tend to converse more often on weekdays, especially on Wednesdays and Thursdays (UTC), that there are three common community structures observed, that developers tend to discuss topics such as API usages and errors, and that six dialog interaction patterns are identified in the live chat communities. 
Based on the findings, we provide recommendations for individual developers and OSS communities,
highlight desired features for platform vendors, and shed light on future research directions. We believe that the findings and insights will enable a better understanding of developers' live chat, pave the way for other researchers, as well as a better utilization and mining of knowledge embedded in the massive chat history.

## 2 Study Design
### 2.1 Research Questions
* RQ1 (Communication Habits): Do Gitter communities demonstrate consistent community communication profiles?
* RQ2 (Community Structure): What are the structural characteristics of social networks built from developer live chat data? 
* RQ3 (Dialog Topic): What are the primary topic types frequently discussed by developers in live chat?
* RQ4 (Interaction Pattern): How do developers typically interact with each other in live chat?

### 2.2 Our Dataset
![image](https://github.com/LiveChat2021/LiveChat/blob/main/images/table-ourdata.png)<br>
&ensp;&ensp;The above table shows the statistics about eight selected communities, and the retrieved dataset contains all utterances as of ???2020-11-20???. P refers to the number of participants, D refers to the number of dialogs, and U refers to the number of utterances. The total number of participants for the eight communities is 95,416, accounting for 13% of the totalpopulation in Gitter. Thus, we consider that the eight communities are representative in the Gitter platform.<br>

## 3 Results
### 3.1 Experiment on Dialog Disentanglement
&ensp;&ensp;To analyze the dialogs in a large scale, we experiment with the four state-of-art dialog disentanglement approaches, i.e. BiLSTM model<sup>[1]</sup>, Bert model<sup>[2]</sup>, E2E model<sup>[3]</sup>, and FF model<sup>[4]</sup>. Specifically, we use the manual disentanglement sample data from the previous step as ground truth data, compare and select the best DD model for the purpose of the further analysis in this study. We test the performances of the four approaches on the 749 manually disentangled dialogs, and adopt four widely used clustering metrics for evaluation: Normalized Mutual Information (NMI) , Adjusted Rand Index (ARI) , Shen-F value and F1 score. ARI is a similarity measure between two clusters based on the evaluation to a pairwise basis, while NMI penalizes more on the cluster-level. Shen-F measures how well related utterances are grouped, which is a very common and useful metric for dialogue disentanglement. F1 is the most strict measure which is calculated using the number of perfectly matching conversations.<br>
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/DD-test.png" width="400" alt="dd-test"/></div><br>
&ensp;&ensp;The comparison results are shown in the above figure. The FF approach significantly outperforms the others on disentangling developer live chat. We can see that, FF approach achieves high NMI (avg. 0.74 ) and Shen-F scores (avg. 0.81), and medium-level scores on F1 (avg. 0.47) and ARI (avg. 0.57). Therefore, we select the FF model to disentangle all the utterances of the eight projects. Finally, we use the best FF model to disentangle all the 1,402,894 utterances in chat logs. In total, we obtain 173,278 dialogs.

### 3.2 RQ1: Communication Profile
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/hours.png" width="400" alt="hours"/></div><br>
&ensp;&ensp;This figure compares the distribution of utterances' intensity over 24 hours, across the eight communities. First, we identify the peak hours of each community in red dashed circles, then highlight the time windows based on the peak hours contained in it with the yellow shade. We can see that, there are three windows of peak hours, which are from UTC 9 to 10, 13 to 14, and 18 to 21. In addition, UTC 1 to 6 corresponds to the low chatting-activity hours. Developers are less active in chatting at that time. 
<br><br>
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/days.png" width="400" alt="days"/></div><br>
&ensp;&ensp;This figure shows the distribution of the utterances across different weekdays. We can see that developers chat more on workdays than on weekends (UTC).<br>
<br>
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/timelag.png" width="400" alt="timelag"/></div><br>
&ensp;&ensp;This figure exhibits the distribution of response time calculated from the 173,278 dialogs of the eight communities. The average response time is 220 seconds, the maximum time lag is 1,264 seconds, and the minimum time lag is two seconds. The peak point is (23, 393), which means there are 393 dialogs that got replies in 23 seconds. 
We can see that, the time lag largely increases from 0 to 23 seconds, and descend in a long tail. Eighty percent of the dialogs get first responses in 343 seconds. As reported by a recent study on Stack Overflow, the threshold of fast answers was 439 seconds. In comparison, live chat gets 50% faster ((439-220)/439) replies than the fast answers in Stack Overflow. Therefore, we consider the responses from the live chat are relatively fast.<br>
<br>

| **Answering RQ1: The peak hours for live chat are from UTC 9 to 10, 13 to 14, and 18 to 21, while UTC 1 to 6 is the low-active hours. Developers are more likely to chat on workdays than weekends, especially on Wednesdays and Thursdays (UTC). Moreover, live chat gets 50% faster replies than the fast answers in Stack Overflow.** |
| :-----|

### 3.3 RQ2: Community Structure
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/table-network.png" width="800" alt="network"/></div><br>
&ensp;&ensp;This table shows the social network properties of the eight communities. Init.%, Resp.%, and Both% denote the percentage of developers serving the role of dialog initiators, respondents, and both. Intuitively, we consider that respondents share their knowledge with others, while initiators receive knowledge from others. We can see that, the four communities (Appium, Docker, Gitter, and Ethereum) have a higher percentage (75.04%-81.70%) of dialog initiators and a lower percentage (18.30%-24.96%) of respondents/both. The high percentage of dialog initiators may relate to the applicable nature of the open-source projects, e.g., Ethereum is one of the most widely used open-source blockchain systems, thus there are a large number of users acquiring technical support from live chat. The other four communities (Angular, DL4J, Nodejs, and Typescript) have a higher percentage (29.94%-48.62%) of respondents/both. A possible explanation is that these four projects are more widely used for development purposes, e.g., Angular is a platform for building mobile and desktop web applications, therefore, such communities appear to be knowledge-sharing and collaborative.<br>

<img width="170"></img>
Angular
<img width="330"></img>
Appium
<br>
<img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/RQ1/angular.png" width="400" height="400" alt="angular"/>
<img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/RQ1/appium.png" width="400" height="400" alt="appium"/><br>

<img width="170"></img>
Docker
<img width="330"></img>
Deeplearning4j
<br>
<img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/RQ1/docker.png" width="400" height="400" alt="docker"/>
<img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/RQ1/dl4j.png" width="400" height="400" alt="deeplearning4j"/><br>

<img width="170"></img>
Ethereum
<img width="330"></img>
Gitter
<br>
<img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/RQ1/ethereum.png" width="400" height="400" alt="ethereum"/>
<img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/RQ1/gitter.png" width="400" height="400" alt="gitter"/><br>

<img width="170"></img>
Typescript
<img width="330"></img>
Nodejs
<br>
<img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/RQ1/typescript.png" width="400" height="400" alt="typescript"/>
<img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/RQ1/nodejs.png" width="400" height="400" alt="nodejs"/><br>


&ensp;&ensp;The above figures shows the social network visualizations of the eight communities generated by Gephi. Each node represents one developer, and the edge denotes the dialog relationship between two developers. We color the vertex of the initiator with blue, the vertex of the respondent with white, and the vertex of both roles with orange. In addition, the node???s size indicates its corresponding degree. Based on the observation on community structures, we categorize the eight communities into three groups, consisting of:
* **Polaris network** is a type of highly centralized network where the community is organized around its single focal point.
* **Constellation network** is a type of moderately centralized network where the community is organized around its multiple focal points. 
* **Galaxy network** is a type of decentralized network where all individuals in the community have similar relationships.<br>

&ensp;&ensp;The four communities on the top (i.e., Angular, DL4J, NodeJS, and Typescript) belong to the Constellation network, i.e., moderately centralized network. Three communities(i.e., Appium, Docker, and Gitter) belong to the Polaris network, i.e., a highly centralized network. The remaining Ethereum community belongs to the Galaxy network, i.e., decentralized network.<br>

<br>

| **Answering RQ2: By visualizing the social networks of eight studied communities, we identify three social network structures for developers' live chat. Half of the communities (4/8) are Constellation networks. A minority of the communities (3/8) are Polaris networks. Only one community belongs to the Galaxy network. In comparison, we find that developers in the live chat may have less influence than developers in email in spreading information, but have a more closely connected community than that from email.** |
| :-----|

### 3.4 RQ3: Discussion Topic
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/donut.png" width="450" alt="donut"/></div><br>
&ensp;&ensp;This figure shows the distribution of discussion topics in developer live chat. The figure shows discussion topics in gray and their categories in white, as well as the percentages of the corresponding dialogs. The taxonomy expands outwards from higher-level categories to lower-level categories and topics.<br>
<br><br>
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/table-topic.png" alt="topic"/></div><br>
&ensp;&ensp;This table shows the descriptions of categories. The orange cells are decomposed from "Discrepancy", and the green cells are decomposed from "Conceptual". <br>
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/donut-sort.png" alt="topic"/></div><br>
&ensp;&ensp;The above figure shows the ranking of topics. For each topic, we provide an example from the real-world live chat. <br>

* **API usage: How to know if the keyboard is open or not**<br>
[2016-12-01 11:32:22] &lt;deepakchoudhury&gt; I have a doubt.hide_keyboard is working great but do you know how to know if the keyboard is open or not<br>
[2016-12-01 11:32:38] &lt;deepakchoudhury&gt; for andorid app<br>
[2016-12-01 12:46:38] &lt;pramodnaik&gt; Hi@deepakchoudhury@deepakchoudhury: I don't think u have an option to check if keyboard is opened or not. But u can try catch the hide keyboard. If it throws exception means keyboard is already closed<br>
[2016-12-01 13:53:26] &lt;deepakchoudhury&gt; Thanks@pramodnaik...its working...<br>

* **Error: I getError: No such image**<br>
[2016-11-28 21:25:01] &lt;mikewrighton&gt; I getError: No such image, container or task: a9952be2c74f2c6f3a7ae9faf12dc1576367bba5197fe74c243b637289fb653b<br>
[2016-11-28 21:25:16] &lt;mikewrighton&gt; but an exec id is not an image, container or task<br>
[2016-11-28 21:26:05] &lt;mikewrighton&gt; so docker inspect, as far as I can tell, does not support quering exec ids<br>
[2016-11-28 22:53:10] &lt;dragon788&gt; mikewrighton: I think you are correct, inspect is mostly for containers and images<br>

* **Background Info: If there is some UI and/or orchestrator that allow me to manage multiple cluster in one place**<br>
[2016-11-30 15:11:51] &lt;jdevillard&gt; Hello, I'm new on docker, I would like to know if there is some UI and/or orchestrator that allow me to manage multiple cluster in one place. I understand that there is many Docker UI available but it seems that it allows to manage only one cluster (or maybe I miss something). Indeed my cluster will be hosted on Azure, in different Vnet and I would like a single interface to manage them all. Do you have any idea?<br>
[2016-11-30 15:24:18] &lt;marcelmfs&gt; check rancher.com<br>
[2016-11-30 15:25:30] &lt;jdevillard&gt; interesting thanks@marcelmfs!<br>

* **Unwanted Behavior??? Unexpected balance as 0 for the ethereum walle**<br>
[2017-02-06 13:47:36] &lt;chafey&gt; I just bought some ethereum from coinbase, but etherscan shows the balance as 0 for the ethereum wallet address coinbase has for me.  Any ideas?<br>
[2017-02-06 13:49:29] &lt;3esmit&gt; chafey: Takes a minute to update the balance. Check again in a few minutes.<br>
[2017-02-06 13:51:55] &lt;chafey&gt; Its been about 30 minutes now<br>
[2017-02-06 13:52:53] &lt;chafey&gt; Perhaps coinbase doesn't actually put the ethereum on the network and keeps it internally?<br>
[2017-02-06 13:53:15] &lt;chafey&gt; Maybe I need to transfer it out of coinbase into my own account?<br>
[2017-02-06 13:54:05] &lt;3esmit&gt; chafey: They keep it internally, you need to call for withdraw in the account, I don't know how to do it but you can ask support for coinbase.<br>
[2017-02-06 13:55:24] &lt;chafey&gt; I see how to transfer, just surprised they don't actually put it on the network.<br>

* **Learning: Examples for a docker compose to use with php 7, postgresql, mongo e nodejs**<br>
[2016-11-22 13:20:39] &lt;LuizTibo&gt; Hi guys, I need a docker compose to use with php 7, postgresql, mongo e nodejs. Do you have some example?<br>
[2016-11-22 13:40:58] &lt;marcelmfs&gt; LuizTibo: : [&lt;-LINK-&gt;] <br>

* **Review: Asking for suggestion about Typescript version for compiling RXJS**<br>
[2017-02-26 19:45:11] &lt;Deviad&gt; What Typescript version is suggested for compiling  RXJS ?<br>
[2017-02-26 19:45:20] &lt;Deviad&gt; In order to avoid these issues<br>
[2017-02-26 20:05:23] &lt;crystalbyte&gt; Deviad: You need core-j s or a similar lib to add Promise support.<br>

* **Social chatting: Chatting about web developer bootcamp**<br>
[2017-01-03 23:01:04] &lt;timojarv&gt; Dunno, I went with the web developer bootcamp on udemy, I was very happy with it and it was only 15<br>
[2017-01-03 23:01:15] &lt;timojarv&gt; Now doing advanced react and redux<br>
[2017-01-03 23:01:51] &lt;AlexandreResende&gt; hmm awesome...<br>
[2017-01-03 23:01:58] &lt;AlexandreResende&gt; is this bootcamp good ?<br>
[2017-01-03 23:06:51] &lt;timojarv&gt;  [&lt;-LINK-&gt;] A great value for that kind of money, it goes through front end and back end.<br>
[2017-01-03 23:06:57] &lt;timojarv&gt; and right now it is only 10<br>
[2017-01-03 23:07:56] &lt;AlexandreResende&gt; awesome, thanks for sharing :D<br>
[2017-01-04 00:07:57] &lt;mjago&gt; timojarv: thanks also for the heads-up on that course<br>
[2017-01-04 00:37:57] &lt;IvenOne&gt; ad. everywhere<br>
[2017-01-04 00:38:10] &lt;IvenOne&gt; awesome<br>

* **General development: About microsoft languages**<br>
[2017-01-07 07:29:16] &lt;SISheogorath&gt; I don't know all those microsoft languages tends to have ultra long names for their methods and objects<br>
[2017-01-07 07:29:26] &lt;galvesribeiro&gt; hehehehehe<br>
[2017-01-07 07:30:17] &lt;SISheogorath&gt; same applies to commandline programs :(dirvsls<br>

* **Do not work: Code about starting container not working**<br>
[2016-11-21 14:49:22] &lt;sirroland&gt; How I can start nginx and php on starting container?<br>
[2016-11-21 14:49:44] &lt;sirroland&gt;  [&lt;-CODE-&gt;] not working ((<br>
[2016-11-21 18:34:59] &lt;killerspaz&gt; krzysztof-magosa: @sirrolandif you want/bin/shto be your interpreter just supply CMD as a quoted array as you have; see: [&lt;-LINK-&gt;] <br>
[2016-11-21 18:38:18] &lt;dragon788&gt; sirroland: you'll probably need to put those two commands in a single  .sh file and execute it via cmd<br>
[2016-11-21 18:56:59] &lt;marcfielding1&gt; @sirroland I think you simply need && after the first start so [&lt;-CODE-&gt;] CMD executes a single command so by using && you get it to do both - at least thats my understanding<br>
[2016-11-21 18:57:13] &lt;marcfielding1&gt; although ideally as has already been mentioned a shell script would be a good idea<br>

* **Reliability Issue: Frequent disconnects every now and then**<br>
[2016-12-11 16:14:31] &lt;soumitraj&gt; hi, I just downloaded the MIST clinet and ethereum wallet<br>
[2016-12-11 16:15:01] &lt;soumitraj&gt; but i observe frequent disconnects every now and then<br>
[2016-12-11 16:15:21] &lt;soumitraj&gt; My clock is 15.5 seconds behind<br>
[2016-12-11 16:15:50] &lt;soumitraj&gt; any ideas what can be the reason of disconnects and how do I sync my clock<br>
[2016-12-11 16:24:53] &lt;erastotle&gt; soumitraj: what OS are you running?<br>
[2016-12-11 16:25:30] &lt;soumitraj&gt; Thanks, windows10<br>
[2016-12-11 16:29:50] &lt;erastotle&gt; soumitraj: is your NTP service set correctly?  The following page gives some answers on setting NTP server: [&lt;-LINK-&gt;] <br>
[2016-12-11 16:30:14] &lt;soumitraj&gt; I will check<br>

* **New Features: Is there a way to set folder ownership on mounted volume?**<br>
[2016-11-23 08:44:20] &lt;ShurikAg&gt; hi all<br>
[2016-11-23 08:44:51] &lt;ShurikAg&gt; Is there a way to set folder ownership on mounted volume?<br>
[2016-11-23 08:45:30] &lt;ShurikAg&gt; Somehow, when I change permissions once, it???s kept on my local docker but not on AWS.<br>
[2016-11-23 18:08:05] &lt;lucasjahn&gt; ShurikAg: I have a really similar file permission issue. So upvote for any helpful tipp on this :)<br>

* **Performance Issue:  Implemented word2vec consumes too much resource and my system stop responding**<br>
[2016-12-22 05:54:58] &lt;jageshmaharjan&gt; Anyone, implemented the word2vec and plot in a tsne graph .<br>
[2016-12-22 06:15:27] &lt;agibsonccc&gt; jageshmaharjan:  [&lt;-LINK-&gt;] this is how you save tsne coordinates given a vector<br>
[2016-12-22 06:15:49] &lt;agibsonccc&gt;  [&lt;-LINK-&gt;] shows how our UI works<br>
[2016-12-22 06:20:14] &lt;agibsonccc&gt; jageshmaharjan: actually looking at the new UI..it looks we may need to implement the new tsne yet<br>
[2016-12-22 06:20:20] &lt;agibsonccc&gt; Mind filing an !issue?<br>
[2016-12-22 06:20:21] &lt;raver120&gt; For DL4J issues, click 'New Issue' at [&lt;-LINK-&gt;] - for ND4J, use [&lt;-LINK-&gt;] instead<br>
[2016-12-22 06:21:01] &lt;jageshmaharjan&gt; agibsonccc: ,  i ran the former example before, but when i ran the later example, it consumes too much resource and my system stop responding. :( .Thank you, I will try on different machine, i was in doubt before whether i was doing something silly. :))<br>
[2016-12-22 06:21:02] &lt;raver120&gt; jageshmaharjan: Welcome! Here's a link to Deeplearning4j's Gitter Guidelines, our documentation and other DeepLearning resources online. Please explore these and enjoy! [&lt;-LINK-&gt;] <br>
[2016-12-22 06:21:21] &lt;agibsonccc&gt; yep<br>

* **Design: Talk through some ideal angular structure perhaps in private**<br>
[2016-12-06T12:19:18] &lt;matthewhaworth&gt; Could anyone spare 5 minutes to talk through some ideal angular structure perhaps in private? I could paste it here but I don't want to spam the channel<br>

* **Test/Buid Failure???Having trouble with Ethereum test network**<br>
[2016-12-03 07:45:39] &lt;quekyt&gt; is anyone else having trouble with Ethereum test network? nothing is going through..<br>
[2016-12-04 11:54:04] &lt;im_ankki_twitter&gt; quekyt: I'm working on same lmk if you have any problems regarding test-network<br>

* **API change: Why appium 1.5.3 will not work with my android emulator?**<br>
[2016-12-30 18:24:58] &lt;blueice349&gt; anyone know why appium 1.5.3 will not work with my android emulator? It worked prior to 1.5.3<br>
[2016-12-31 07:52:32] &lt;shabinmohan&gt; blueice349: app is nnot launching ?<br>
[2017-01-02 04:40:27] &lt;blueice349&gt; yes its not I have been trying to firgure out what is wrong<br>
[2017-01-02 04:40:32] &lt;blueice349&gt; it works on Android Device<br>

<br>

| **Answering RQ3: Developers launch solution-oriented dialogs and problem-oriented dialogs more than knowledge-oriented dialogs. Nearly 1/3 of dialogs are about API usage. Developers discuss more error, unwanted behavior, and do-not-work, than reliability issues, performance issues, and test/build failures.** |
| :-----|

### 3.5 RQ4: Interactive Pattern
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/patterns.png" width="600" alt="patterns"/></div><br>
&ensp;&ensp;This figure illustrates the six interaction patterns in live chat, constructed using open card sorting. This figure shows dialog initiators in blue nodes, respondents in yellow nodes. The lines denote the reply-to relationships, and the labels represent developer intents in this table.<br>
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/table-intent.png" alt="intent"/></div><br>
&ensp;&ensp;In this work, we identify the following six interaction patterns:<br>

* **P1: Exploring Solutions.** Given the original questions posted by the dialog initiator, other developers provide possible answers. But the initiator gives negative feedback indicating these answers do not address the question. When the correct answer is posted, the initiator gives positive feedback and ends the dialog. 
* **P2: Clarifying Answer.** Given the original questions posted by the dialog initiator, another developer provides a possible answer. Then the initiator posts follow-up questions to clarify the answer until the initiator fully understands. 
* **P3: Clarifying Question.** Given the original questions posted by the dialog initiator, the respondent requires the initiator to clarify the question in more detail until they fully understand. Then the respondent posts an answer, and the initiator gives feedback or greetings.
* **P4: Direct/Discussed Answer.** Given the original questions posted by the dialog initiator, the respondent directly answers, or answers after an internal discussion.    
* **P5: Self-answered Monologue.** The original questions posted by the dialog initiator are answered by himself or herself. 
* **P6: Unanswered Monologue.** The original questions posted by the dialog initiator are not answered. 

<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/topic-pattern.png" width="600" alt="topic-pattern"/></div><br>
&ensp;&ensp;The above figure shows the percentage of interaction patterns in different communities, and the average percentages are shown in the legends. We can see that the direct/discussed answer (P4) pattern takes the largest proportions in most communities. In addition, we note that quite a few dialogs (1%) belong to self-answered monologue, while 24% of dialogs belong to unanswered monologue. Nearly 1/4 of dialogs did not get responses in live chat.<br>
<br>
<div align=center><img src="https://github.com/LiveChat2021/LiveChat/blob/main/images/duration.png" width="600" alt="duration"/></div><br>
&ensp;&ensp;The above figure shows the violin plots with the distribution of duration for each pattern. P1-P5 refer to interaction patterns defined above. Here we only exhibit five patterns because the P6 refers to unanswered monologues which barely have a duration. We can see that although P1 takes a small proportion in dialogs, it lasts the longest. Its average duration is 1.00 hours. P2 and P3 last slightly longer than P4. P5 lasts the shortest, and its average duration is 0.02 hour. <br>
<br>

| **Answering RQ4: Six interaction patterns are identified in live chat: exploring solutions, clarifying answer, clarifying question, direct/discussed answer, self-answered monologue, and unanswered monologue. The direct/discussed answer pattern takes the largest proportions in most communities. There are still 1/4 dialogs that did not get responses on average. Dialogs that belong to the Exploring Solutions pattern last the longest time than others.** |
| :-----|

## 4 Conclusion
&ensp;&ensp;In this paper, we have presented the first large-scale study to gain an empirical understanding of OSS developers' live chat. Based on 173,278 dialogs taken from eight popular communities on Gitter, we explore the temporal communication profiles of developers, the social networks and their properties towards the community, the taxonomy of discussion topics, and the interaction patterns in live chat. Our study reveals a number of interesting findings including: <br>
1. There are three social patterns in the OSS community of live chat: polaris network, constellation network, and galaxy network. Constellation networks are the most, followed by polaris networks, galaxy networks are the least;
2. Developers are more likely to chat in workdays than weekends, especially in Wednesday and Thursday (UTC);
3. Developers are more active from UTC 9 to 10, 13 to 14, and 18 to 21, corresponding to Central European/American day time or Asia night time. The low-active time slice for chatting is Central European/American night time or Asia day time;
4. Nearly 1/3 dialogs are about API usage. Developers discuss more about errors, unwanted behaviors and do-not-work, than reliability issues, performance issues, and test/build failures;
5. There are six interaction patterns identified in live chat: exploring solution, clarifying answer, clarifying question, direct/discussed answer, self-answered monologue, and unanswered monologue;
6. We provide guidelines for developers in live chat, highlight advanced features for online communication platform vendors, and provoke insightful future research questions for OSS researchers. In future, we plan to investigate how well can we automatically classify the dialogs into different topics, as well as attempt to construct knowledge bases according to already answered questions and their corresponding solutions from live chat.<br>
<br>
&ensp;&ensp;Moreover, we provide recommendations for both OSS developers and communities, highlight advanced features for online communication platform vendors, and provoke insightful future research questions for OSS researchers. In the future, we plan to investigate how well can we automatically classify the dialogs into different topics, as well as attempt to construct knowledge bases according to already answered questions and their corresponding solutions from live chat. We hope that the findings and insights that we have uncovered will pave the way for other researches, help drive a more in-depth understanding of OSS development collaboration, and promote a better utilization and mining of knowledge embedded in the massive chat history. To facilitate replications or other types of future work, we provide the utterance data and disentangled dialogs used in this study online: https://github.com/LiveChat2021/LiveChat}{https://github.com/LiveChat2021/LiveChat.

## 5 Download
[Original chat history (Download all)](https://github.com/LiveChat2021/LiveChat/blob/main/data/raw.zip?raw=true): including **`1,402,894`** utterances of the eight Gitter communites, from Angular to Typescript. 
<br>
| **Project** | <img width="40"></img>**Size(kb)**<img width="40"></img>      | **Download** |
| :----:| :----: | :----: |
| Angular | &nbsp;&nbsp;&nbsp;87,629&nbsp;&nbsp;&nbsp; | [Download](https://github.com/LiveChat2021/LiveChat/blob/main/data/raw/angular_angular_chatFormat.ascii.txt?raw=true) |
| Appium | 3,475 | [Download](https://github.com/LiveChat2021/LiveChat/blob/main/data/raw/appium_appium_chatFormat.ascii.txt?raw=true) |
| Deeplearning4j | 36,458 | [Download](https://github.com/LiveChat2021/LiveChat/blob/main/data/raw/eclipse_deeplearning4j_chatFormat.ascii.txt?raw=true) |
| Docker | 3,543 | [Download](https://github.com/LiveChat2021/LiveChat/blob/main/data/raw/docker_docker_chatFormat.ascii.txt?raw=true) |
| Ethereum | 7,858 | [Download](https://github.com/LiveChat2021/LiveChat/blob/main/data/raw/ethereum_welcome_chatFormat.ascii.txt?raw=true) |
| Gitter | 1,894 | [Download](https://github.com/LiveChat2021/LiveChat/blob/main/data/raw/gitter_gitter_chatFormat.ascii.txt?raw=true) |
| Nodejs | 11,029 | [Download](https://github.com/LiveChat2021/LiveChat/blob/main/data/raw/nodejs_node_chatFormat.ascii.txt?raw=true) |
| Typescript | 22,532 | [Download](https://github.com/LiveChat2021/LiveChat/blob/main/data/raw/Microsoft_Typescript_chatFormat.ascii.txt?raw=true) |
<br>

[Automatically disentangled dialogs (Download all)](https://github.com/LiveChat2021/LiveChat/blob/main/data/disentangle.zip?raw=true): including **`173,278`** dialogs disentangled by FF model. 
<br>
| **Project** | **Number of Dialogs** | **Download** |
| :----:| :----: | :----: |
| Angular | 79,619 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/disentangle/angular_angular.txt) |
| Appium | 4,906 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/disentangle/appium_appium.txt) |
| Deeplearning4j | 27,256 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/disentangle/eclipse_deeplearning4j.txt) |
| Docker | 3,954 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/disentangle/docker_docker.txt) |
| Ethereum | 17,298 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/disentangle/ethereum_welcome.txt) |
| Gitter | 7,452 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/disentangle/gitter_gitter.txt) |
| Nodejs | 13,981 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/disentangle/nodejs_node.txt) |
| Typescript | 18,812 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/disentangle/Micro_Typescript.txt) |
<br>

[Manually disentangled dialogs (Download all)](https://github.com/LiveChat2021/LiveChat/blob/main/data/manual.zip?raw=true): including **`749`** dialogs disentangled in a manual way.
<br>
| **Project** | **Number of Dialogs** | **Download** |
| :----:| :----: | :----: |
| Angular | 97 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/manual/angular.txt) |
| Appium | 87 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/manual/Appium.txt) |
| Deeplearning4j | 100 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/manual/dl4j.txt) |
| Docker | 90 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/manual/docker.txt) |
| Ethereum | 96 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/manual/ethereum.txt) |
| Gitter | 86 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/manual/Gitter.txt) |
| Nodejs | 98 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/manual/nodejs.txt) |
| Typescript | 95 | [Download](https://raw.githubusercontent.com/LiveChat2021/LiveChat/main/data/manual/Typescript.txt) |
<br>

## 6 References
[1] Gaoyang Guo, Chaokun Wang, Jun Chen, and Pengcheng Ge. 2018. Who IsAnswering to Whom? Finding ???Reply-To??? Relations in Group Chats with LongShort-Term Memory Networks. InProceedings of the 7th International Conferenceon Emerging Databases.<br>
[2] Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. 2019. BERT:Pre-training of Deep Bidirectional Transformers for Language Understanding. InProceedings of the 2019 Conference of the North American Chapter of the Associationfor Computational Linguistics: Human Language Technologies, NAACL-HLT 2019,Volume 1 (Long and Short Papers). Association for Computational Linguistics,4171???4186.<br>
[3] Hui Liu, Zhan Shi, Jia-Chen Gu, Quan Liu, Si Wei, and Xiaodan Zhu. 2020. End-to-End Transition-based Online Dialogue Disentanglement. InProceedings of theTwenty-Ninth International Joint Conference on Artificial Intelligence, IJCAI 2020.ijcai.org, 3868???3874.<br>
[4] Jonathan K. Kummerfeld, Sai R. Gouravajhala, Joseph Peper, Vignesh Athreya,Chulaka Gunasekara, Jatin Ganhotra, Siva Sankalp Patel, Lazaros Polymenakos,and Walter S. Lasecki. 2019. A Large-scale Corpus for Conversation Disen-tanglement. InProceedings of the 57th Annual Meeting of the Association forComputational Linguistics (Volume 1: Long Papers).<br>
