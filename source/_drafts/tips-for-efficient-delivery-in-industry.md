---
title: Tips for Efficient Delivery in Industry
tags:

categories:
- Misc
---

In this post, I would like to share my thoughts on efficiently delivering things in the industry, from the perspective of a 1 YOE individual contributor (IC). I won't delve into explaining how to learn new technologies and technical details since they are not the main focus of this post.

The content is divided into three main sections:

1. What you should do before working on the task
2. What you should do while working on the task
3. What you should do after you finish the task

---

**Content**

- [Before working on the task](#before-working-on-the-task)
- [While working on the task](#while-working-on-the-task)
- [After finishing the task](#after-finishing-the-task)
- [References](#references)

## Before working on the task

+ Clear about the scope of ticket, what's required and what's not. Do not work on those beyond the requirements. Even you do think necessary, create extra tickets to embrace and break down the work into managable pieces

+ Estimate the needed efforts to tackle the task and have a good schedule for your tasks. Do not overburden yourself but also make sure to deliver based your ability. Things are case-by-case to say. An experienced engineer may only need 1 day for a task while a fresh new engineer may need more than 1 week for the same amount of work. From my personal experience, when I just started to work, everything seems to be a big monster for me, even a small task such as adding a CI/CD pipeline that is simple enough for me now, I spent a week or more to tackle that small piece of task when I just got into the industry and have almost no knowledge of those tech stack. To summarize, make sure you have a good estimate of a good estimate of the efforts needed to get the task done based on your capacity and ability



## While working on the task

+ First and most importantly. Don't write huge PRs. No one wants to look at a super big PR. Always think about how you can break a big task into a set of features with small PRs. (Like the principle of microservices, usually we break down a singleton service into self-contained services so that each of them is easy to develop and maintain). Singleton is evil, as is the same case here when working some big features.

+ Write meaningful commit messages, it's good follow conventional commit
  
+ Write clean code. If a new feature, write comments, use meanful names. This should have been by many books and language style guides so I won't expand on this. It's the basic and essential quality of every qualified engineer. If you don't write clean code, people may find it hard to work with you.

+ Code review, there's a lot to say about code reviews. Also this is where I get many experiences from. I've requested so many code reviews and also review many PRs during the past year


## After finishing the task

+ Review and ask for some feedbacks and think about how you can do better or even improve on some small point. If you fail to deliver the results to the expectation, think about why, it's because time, blocked by something else, or by your procrastination

+ Able to explain to people what you have done on this and how it could help people working on similar things with this new feature or fix patch. Make sure everyone in your team understands what you did so that it's easy for others deliver work based on your progress. Otherwise if a single piece of component is only understandable by a single person and almost eveyone else struggles make progress based on it, it's really not a good idea and ideal design.

+ Communicating your progress with your manager and tech lead. Worth to mention this a bit, when talking and syncing to your manager, don't delve into small technical details since that's usually not your manager cares about and can chime into. Step backwards and look into the big picture and efficiently communicate what you have achieved with your manager. While things are quite different when communicating with your tech lead or other teammates, you should mention important technical details since they also need to understand it. They need evidence and solid understanding of why you do things this way while not another


## References

+ [*Clean Code*](https://book.douban.com/subject/3032825/)
+ [*Refactoring*](https://book.douban.com/subject/1419359/)
+ [*Head First Design Patterns*](https://book.douban.com/subject/1400656/)
