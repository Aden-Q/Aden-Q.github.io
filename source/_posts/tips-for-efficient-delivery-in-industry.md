---
title: Tips for Efficient Delivery in Industry
categories:
  - Misc
date: 2024-02-15 02:07:34
tags:
---


In this post, I would like to share my thoughts on efficiently delivering results in the industry, from the perspective of a 1-year experienced individual contributor (IC). I won't delve into explaining how to learn new technologies and technical details since they are not the main focus of this post. Additionally, I'm still on my way to becoming a good engineer, so some of the content may not be mature or universally applicable, and my perspectives may evolve as I gain more experience.

The content is divided into three main sections:

1. What you should do before working on the task
2. What you should do while working on the task
3. What you should do after you finish the task

---

**Content**

- [Before working on the task](#before-working-on-the-task)
- [While working on the task](#while-working-on-the-task)
- [After finishing the task](#after-finishing-the-task)
- [Final Tips](#final-tips)
- [References](#references)

## Before working on the task

1. **Clarify ticket scope and requirements.** Essentially be clear about what's required and what's not. Avoid working beyond scope. Create additional tickets as needed. If any requirement is unclear, don't hesitate to ask for clarifications. It's more effective to ensure everything is clear before working on the implementation rather than completing the task and having to refactor due to unclear or misunderstood requirements.

2. (For coding tasks) **Tech stack consensus.** In the Go Programming realm, there are so many choices of frameworks to use from the open source community. And there are usually tradeoffs. Don't always stick with only one tool and framework. Think outside the box and choose the right tool to use so that you don't have to waste time on refactoring everything later on, which can sometimes be a huge pain."

3. **Estimate the efforts to tackle the task and create a good schedule.** This varies case by case. An experienced engineer may need less time than an inexperienced one for the same task. From my personal experience, when I just started working, even a seemingly simple task like adding a CI/CD pipeline, which is straightforward for me now, took me a week or more. Ensure you have a realistic estimate of the efforts needed based on your capacity and ability. A good estimate also means avoiding overburdening yourself.

## While working on the task

1. (For coding tasks) **Don't write huge PRs.** It's one of the most important lessons I've learned. No one wants to look at a super big PR. Always think about how you can break it into manageable small pieces. This will not only make PRs much easier for others to review but also make troubleshooting potential bugs a lot easier. Analogous to the principle of microservices, we usually break down a singleton service into self-contained services so that each of them is easy to develop and maintain.

2. (For coding tasks) **Always write meaningful commit messages.** It's a good practice to follow [conventional commit guidelines](https://www.conventionalcommits.org/en/v1.0.0/).

3. (For coding tasks) **Write clean code.** It's about coding style, comments, desgn patterns, etc. I won't expand on this point simply because books in the [reference section](#references) have done a much better job of explaining this. It's the basic and essential quality of every qualified engineer. If you don't write clean code, people may find it hard to work with you.

4. (For coding tasks) **Code review.** There's much to share about code reviews based on my personal experience. Over the past year, I've both requested numerous code reviews and reviewed many PRs. Firstly, avoid asking for reviews right before the end of the day to respect people's personal time. If a timely review is not possible for any reason, shift your focus to something else. If someone forgets to review your PR, kindly remind them. Secondly, when you've made attempts and are stuck, don't spend an infinite amount of time on it; ask your teammates for help. In my own experience, as a new hire, exposure to new services and tech stacks sometimes left me feeling overwhelmed. Having good teammates and leaders who were willing to help and provide guidance was invaluable. Thirdly, defend yourself when necessary. Don't blindly accept review comments and advice. If you believe you are right, speak up and defend your position, but do so politely and concisely. Developing communication skills is another important lesson to learn.

## After finishing the task

1. **Reviews and retrospective.** Seek feedback and contemplate how you can enhance or improve even on minor aspects. If you fall short of meeting expectations, analyze whether it's due to time constraints, external obstacles, or procrastination.

2. **Effectively communicate your progress with your manager and tech lead.** It's worth mentioning that when discussing and syncing up with your manager, avoid delving into small technical details, as those are usually not their primary focus or area of expertise. Step back and provide a high-level overview, efficiently communicating what you have achieved. On the other hand, when communicating with your tech lead or other teammates, highlighting important technical details is crucial, as they need a comprehensive understanding of your decisions. Providing evidence and clarity on why you chose a particular approach over another is essential in these discussions.

## Final Tips

1. Stay humble. Resist losing yourself in praise. Keep moving forward.

2. Make good planning and don't be bitten by procrastination, which is a true obstacle in any case. As mentioned in [this post](https://aden-q.github.io/Learning-How-to-Learn-Course-Notes/).

3. Recharge yourself after working hours. I'm not encouraging excessive competition or any form of burnout, as smart individuals know how to maximize their time and work efficiently.

4. Don't get trapped in all the subtle technical details specific to certain languages, frameworks, or tools. However, it's worth noting that as individual contributors, there are always things to learn along our journey, and fast learning ability is crucial. Technologies keep evolving, and there's always something new. You can't possibly learn every technology, but it's essential to have an awareness of their existence and understand where they can be applied.

5. Think in big pictures and adopt system design views. Consider why we design things in a certain way and how we can improve them, instead of just completing tasks assigned by leaders (which anyone with some experience can do).

6. I give up giving universal tips since it's really case by case...

## References

- [*Clean Code*](https://book.douban.com/subject/3032825/)
- [*Refactoring*](https://book.douban.com/subject/1419359/)
- [*Head First Design Patterns*](https://book.douban.com/subject/1400656/)
