# System Design Interview Tips

System design interviews are typically part of the interview process for senior-level software engineers and above. These interviews are open-ended discussions in which the candidate is asked to architect something, usually a popular product or service. For example, you may be asked to design Twitter, YouTube, or Google Docs.

Of course, 30–60 minutes isn’t nearly enough time to design any of these systems. The real products have been built over many years by thousands of developers working full-time.

So, what are you expected to actually do during your system design interview? We can break it down into four steps:

1. Ask Questions and Establish Scope
2. Create a High-Level Design
3. Do a Deep Dive into a Few Components
4. Wrap Up and Discuss Further Improvements

Let’s consider each of them below.

---

## Ask Questions and Establish Scope

The biggest mistake you can make during a system design interview is to jump right into the design without first asking questions. Because system design interview questions are intentionally vague, you’ll want to collaborate with your interviewer to establish design scope and validate any assumptions you may have.

You’ll likely want to ask some or all of the following questions:

* **Which features are most important?** (For example, designing Twitter is no small feat, so the interviewer may tell you to focus on just the news feed.)

* **How many daily active users are there?** (It’s important to know if this a v1 product for a tiny startup or an established product that has millions of users.)

* **Can I leverage existing cloud providers like AWS, GCP, or Azure?** (Sometimes they want you to build something from scratch, but most of the time it’s perfectly acceptable to say that you’ll use existing infrastructure like a load balancer (ELB), auto-scaling groups, document storage (S3), message queues (SQS), cache (Redis), or CDN (CloudFront).

* **What types of data are we dealing with?** (It’s good to know if you’re just working with text or if you’re storing images or videos as part of your service. The data type and data structure will likely influence your choice of database.)

* **Do we need to consider [X]?** (The beginning of the interview is all about establishing scope, so ask your interviewer about various concerns that should be considered in scope or out of scope. Oftentimes they’ll let you stick with the simpler approach to begin with and then tackle the harder problems toward the end of the interview if there’s time.)

* **Can I assume that [X]?** (Make your assumptions known to your interviewer. This will ensure that you’re both on the same page. The interviewer may also provide additional clarification for you here based on your previous assumptions.)

Once you feel that you have a decent understanding of the problem, you can move on to the high-level design.

---

## Create a High-Level Design

During this second step, you should work with your interviewer to design the system at a high level. Don’t dive too deep into a single component for now. Instead, stick to a high-level overview of how the system would work. At this time, it’s not necessary to go into your database schema or what API endpoints you’d implement.

By staying at a high level to begin with, you can validate your ideas with your interviewer. You can bounce ideas off of your interviewer and get their input. During a system design interview you should treat your interviewer like a collaborator or a coworker. After all, you’ll likely be working with them if you get the job.

It’s important to remember that even though this interview is about your technical design skills, it’s also about your communication skills.

Can you justify why you’d use one type of database over another? Can you explain how you’re making your app scalable? Can you discuss tradeoffs in CAP theorem when it comes to consistency, availability, and partition tolerance? Have you thought about how you would handle system failures? And when your interviewer gives you feedback or asks you to consider a different approach, do you respond well? A good interviewer will be looking for all of these signals throughout the interview.

By the end of this step, you should have a system that you and the interviewer have agreed upon.

---

## Do a Deep Dive Into a Few Components

Now it’s time to go deep. Again, the system you’re designing is likely a replica of a popular product, so it’s impossible to cover everything in the short amount of time you have during your interview. It’s a good idea to ask your interviewer where they’d like to focus next. Or, suggest a few areas you’d like to discuss more in depth.

For example, if designing Google Docs, how would you handle conflicts when multiple people are editing the same document at the same time?

Or, when building YouTube, how would implementing a message queue help decouple your system components when users upload new videos?

You already have your high-level design, so it’s during this part of the interview that you can showcase the depth of your knowledge on how the infrastructure works.

---

## Wrap Up and Discuss Further Improvements

During the last part of the interview, you can discuss further optimizations you would make if you had more time. You might discuss how you’d handle various edge cases, any performance bottlenecks in your design, or how you’d scale the system further as the app grows.

This is a great time to showcase your thoughtfulness.

---

## Conclusion

Here are those four steps again for a system design interview:

1. Ask Questions and Establish Scope
2. Create a High-Level Design
3. Do a Deep Dive into a Few Components
4. Wrap Up and Discuss Further Improvements

Following this template during your interview will help you stay on track without getting lost in the weeds. Now good luck on your next interview!
