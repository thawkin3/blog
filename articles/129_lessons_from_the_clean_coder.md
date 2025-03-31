# Lessons from The Clean Coder

I read _The Clean Coder_ by Robert C. Martin back in 2022, and I remember feeling like it was chock-full of wisdom.

To avoid confusion, this is not the book _Clean Code_ by the same author. _Clean Code_ focuses mostly on coding itself and hard skills for writing good code. _The Clean Coder_ is a followup book and focuses on soft skills — how we as developers can become true professionals and craftsmen.

Some of the advice in this book is controversial. The piece that stands out most to me is the statement that developers should be spending 60 hours per week on their craft — 40 hours at work and 20 hours learning outside of work. That may not be realistic for many developers, especially those with obligations to their family, community, or elsewhere. The underlying sentiment is useful though, which is that the best developers invest in themselves and are continuously learning. It’s up to you to decide what that looks like in your life.

When I read tech books, I always take notes and highlight passages that resonate with me. It helps me retain the information I found meaningful. I’ve reproduced most of my notes below, and I hope that you’ll find something that resonates with you as well.

Enjoy!

## Preface

Managers should not overrule the expert opinions of those they lead, especially when their direct reports have thought about the problem longer, have more context, and have more relevant experience and expertise in the area.

## Chapter 1: Professionalism

“You cannot simply keep making the same errors over and over. As you mature in your profession, your error rate should rapidly decrease towards the asymptote of zero. It won’t ever get to zero, but it is your responsibility to get as close as possible to it.”

“It is unprofessional in the extreme to purposely send code that you know to be faulty to QA.”

“Every single line of code that you write should be tested. Period.”

“Why do most developers fear to make continuous changes to their code? They are afraid they’ll break it. Why are they afraid they’ll break it? Because they don’t have tests. It all comes back to the tests.”

“Your career is your responsibility. It is not your employer’s responsibility to make sure you are marketable. It is not your employer’s responsibility to train you, or to send you to conferences, or to buy you books. These things are your responsibility. Woe to the software developer who entrusts his career to his employer.” (He then goes on to say that some employers do provide training or conferences or learning budgets for you, and you should be grateful when they do.)

“You should plan on working 60 hours per week. The first 40 are for your employer. The remaining 20 are for you. During this remaining 20 hours you should be reading, practicing, learning, and otherwise enhancing your career. … Perhaps you think that work should stay at work and that you shouldn’t bring it home. I agree! You should not be working for your employer during those 20 hours. Instead, you should be working on your career. … During that 20 hours you should be doing those things that reinforce that passion. Those 20 hours should be fun!”

At a minimum, every software professional should be familiar with design patterns (gang of four), design principles (SOLID), methods (Waterfall, Agile, Scrum, Kanban), disciplines (TDD, OOP, pair programming, CI/CD), and artifacts (UML, flow charts).

“The best way to learn is to teach.”

“It is the responsibility of every software professional to understand the domain of the solutions they are programming. … It is the worst kind of unprofessional behavior to simply code from a spec without understanding why that spec makes sense to the business. Rather, you should know enough about the domain to be able to recognize and challenge specification errors.”

## Chapter 2: Saying No

“Professionals speak truth to power. Professionals have the courage to say no to their managers.”

“A team player is not someone who says yes all the time.”

Stick to your estimates if you have reason to believe they are accurate, even if you are under pressure to commit to finishing at an earlier date. Estimates, targets, and commitments are three different things. Unless you have a radical plan to change how you work, your priorities, or the scope of the work, committing to an earlier deadline than you believe you can finish in is unprofessional.

## Chapter 3: Saying Yes

“There are three parts to making a commitment. 1) You say you’ll do it. 2) You mean it. 3) You actually do it.”

“There are very few people who, when they say something, they mean it and then actually get it done. There are some who will say things and mean them, but they never get it done. And there are far more people who promise things and don’t even mean to do them.”

“You can only commit to things that you have full control of. For example, if your goal is to finish a module that also depends on another team, you can’t commit to finish the module with full integration with the other team. But you can commit to specific actions that will bring you to your target.”

“If you can’t make your commitment, the most important thing is to raise a red flag as soon as possible to whoever you committed to.”

“Professionals are not required to say yes to everything that is asked of them. However, they should work hard to find creative ways to make ‘yes’ possible.”

## Chapter 4: Coding

Being able to sense your errors and having a fast feedback loop is extremely important.

“Often the customer’s requirements do not actually solve the customer’s problems. It is up to you to see this and negotiate with the customer to ensure that the customer’s true needs are met.”

It’s difficult to write code when you’re worried about something else outside of work, like problems at home or a fight with your spouse. Don’t try to code during these times. Seek instead to resolve the problem so your worries go away. Then you’ll be able to focus again.

“It is incumbent upon you as a professional to reduce your debugging time as close to zero as you can get. Clearly zero is an asymptotic goal, but it is the goal nonetheless.”

“I have solved an inordinate number of problems in the shower. … Sometimes the best way to solve a problem is to go home, eat dinner, watch TV, go to bed, and then wake up the next morning and take a shower.”

When working on a project or task, “regularly measure your progress against your goal, and come up with three fact-based end dates: best case, nominal case, and worst case.”

“Of all the unprofessional behaviors that a programmer can indulge in, perhaps the worst of all is saying you are done when you know you aren’t.”

“You avoid the problem of false delivery by creating an independent definition of ‘done.’”

“Learn how to ask for help. When you are stuck, or befuddled, or just can’t wrap your mind around a problem, ask someone for help. … It is unprofessional to remain stuck when help is easily accessible.”

“Nothing can bring a young software developer to high performance quicker than his own drive, and effective mentoring by his seniors.”

## Chapter 5: Test Driven Development

“The bottom line is that TDD works, and everybody needs to get over it.”

Follow the “red, green, refactor” cycle.

“When you have a suite of tests that you trust, then you lose all fear of making changes.”

“Writing your tests first creates a force that drives you to a better decoupled design.”

## Chapter 6: Practicing

“The purpose of learning a kata [in martial arts] is not to perform it on stage. The purpose is to train your mind and body how to react in a particular combat situation. The goal is to make the perfected movements automatic and instinctive so that they are there when you need them.”

“[Programming katas are] a good way to drive common problem/solution pairs into your subconscious, so that you simply know how to solve them when facing them in real programming.”

“Practicing is what you do when you aren’t getting paid. You do it so that you will be paid, and paid well.”

## Chapter 7: Acceptance Testing

On changing requirements: “The problem is that things appear different on paper than they do in a working system. When the business actually sees what they specified running in a system, they realize that it wasn’t what they wanted at all. Once they see the requirement actually running, they have a better idea of what they really want and it’s usually not what they are seeing.”

“The purpose of acceptance tests is communication, clarity, and precision. By agreeing to them, the developers, stakeholders, and testers all understand what the plan for the system behavior is.”

“Acceptance tests should always be automated. There is a place for manual testing elsewhere in the software lifecycle, but these kinds of tests should never be manual. The reason is simple: cost. … The cost of automating acceptance tests is so small in comparison to the cost of executing manual test plans that it makes no economic sense to write scripts for humans to execute.”

“Typical business analysts write the ‘happy path’ versions of the tests, because those tests describe the features that have business value. QA typically writes the ‘unhappy path’ tests, the boundary conditions, exceptions, and corner cases. This is because QA’s job is to help think about what can go wrong.”

“Acceptance tests are not unit tests. Unit tests are written by programmers for programmers. They are formal design documents that describe the lowest level structure and behavior of the code. The audience is programmers, not business. Acceptance tests are written by the business for the business (even when you, the developer, end up writing them). They are formal requirements documents that specify how the system should behave from the business’ point of view. The audience is the business and the programmers.”

“Unit tests and acceptance tests are documents first, and tests second. Their primary purpose is to formally document the design, structure, and behavior of the system. The fact that they automatically verify the design, structure, and behavior that they specify is wildly useful, but the specification is their true purpose.”

## Chapter 8: Testing Strategies

“Professional developers test their code.”

Follow the testing pyramid.

“Unit tests provide as close to 100% coverage as is practical. Generally this number should be somewhere in the 90s. And it should be true coverage as opposed to false tests that execute code without asserting the behavior.”

“Component tests cover roughly half the system. They are directed more towards happy-path situations and very obvious corner, boundary, and alternate-path cases. The vast majority of unhappy-path cases are covered by unit tests and are meaningless at the level of component tests.”

“Integration tests are typically not executed as part of the Continuous Integration suite, because they often have longer runtimes. Instead, these tests are run periodically (nightly, weekly, etc.) as deemed necessary by their authors.”

“System tests cover perhaps 10% of the system. This is because their intent is not to ensure correct system behavior, but correct system construction. The correct behavior of the underlying code and components have already been ascertained by the lower layers of the pyramid.”

“[Manual exploratory tests] are not automated, nor are they scripted. … Creating a written test plan for this kind of testing defeats the purpose.”

## Chapter 9: Time Management

“Sometimes a meeting will be about something that interests you, but is not immediately necessary. You will have to choose whether you can afford the time. Be careful — there may be more than enough of these meetings to consume your days.”

“Sometimes you find yourself siting in a meeting that you would have declined had you known more. … Remaining in a meeting that has become a waste of time for you, and to which you can no longer significantly contribute, is unprofessional.”

“Iteration planning meetings are meant to select the backlog items that will be executed in the next iteration. Estimates should already be done for the candidate items. Assessment of business value should already be done. In really good organizations the acceptance/component tests will already be written, or at least sketched out.”

You can only intensely focus for so long. Manage your time wisely so that you are coding during high-focus times and doing more trivial tasks during other times.

“[Professionals] never become so vested in a solution that they can’t abandon it.”

## Chapter 10: Estimation

“Business likes to view estimates as commitments. Developers like to view estimates as guesses. The difference is profound.”

“An estimate is not a number. An estimate is a distribution.”

“Professionals draw a clear distinction between estimates and commitments. They do not commit unless they know for certain they will succeed.”

PERT (Program Evaluation and Review Technique): You provide three numbers for your estimate — optimistic, nominal (most likely), and pessimistic.

“Software professionals are very careful to set reasonable expectations despite the pressure to try to go fast.”

“[When estimating tasks as a team,] agreement does not need to be absolute. As long as the estimates are close, it’s good enough. So, for example, a smattering of 3s and 4s is agreement. However if everyone holds up 4 fingers except for one person who holds up 1 finger, then they have something to talk about.”

“The simultaneity of displaying fingers is important. We don’t want people changing their estimates based on what they see other people do.”

“If you break up a large task into many smaller tasks and estimate them independently, the sum of the estimates of the small tasks will be more accurate than a single estimate of the larger task.”

“Errors in estimates tend toward underestimation and not overestimation.”

“Breaking the tasks up is a good way to understand those tasks better and uncover surprises.”

## Chapter 11: Pressure

“You know what you believe by observing yourself in a crisis. If in a crisis you follow your disciplines, then you truly believe in those disciplines. On the other hand, if you change your behavior in a crisis, then you don’t truly believe in your normal behavior.”

“Slow down. Think the problem through.”

## Chapter 12: Collaboration

“We didn’t become programmers because we like working with people.”

“It is far better to break down all walls of code ownership and have the team own all the code. I prefer teams in which any team member can check out any module and make any changes they think are appropriate. I want the team to own the code, not the individuals.”

“Although all team members have a position to play, all team members should also be able to play another position in a pinch.”

“Perhaps you believe that you work better when you work alone. That may be true, but it doesn’t mean that the team works better when you work alone. … There are times when working alone is the right thing to do. There are times when you simply need to think long and hard about a problem. There are times when the task is so trivial that it would be a waste to have another person working with you. But, in general, it is best to collaborate closely with others and to pair with them a large fraction of the time.”

## Chapter 13: Teams and Projects

“It makes no sense to tell a programmer to devote half their time to project A and the rest of their time to project B, especially when the two projects have two different project managers, different business analysts, different programmers, and different testers.”

“It takes time for a team to work out their differences, come to terms with each other, and really gel. It might take six months. It might even take a year. But once it happens, it’s magic. A gelled team will plan together, solve problems together, face issues together, and get things done. Once this happens, it’s ludicrous to break it apart just because a project comes to an end. It’s best to keep that team together and just keep feeding it projects.”

“Teams are harder to build than projects. Therefore, it is better to form persistent teams that move together from one project to the next and can take on more than one project at a time. The goal in forming a team is to give that team enough time to gel, and then keep it together as an engine for getting many projects done.”

## Chapter 14: Mentoring, Apprenticeship, and Craftsmanship

“I’e noticed that [software engineers who do really well] have something in common: Nearly all of them taught themselves to program before they entered university and continued to teach themselves despite university.”

“Even the best CS degree programs do not typically prepare the young graduate for what they will find in industry. … What you learn in school and what you find on the job are often very different things.”

“Companies lose huge amounts of money due to the inadequate training of their software developers. Somehow the software development industry has gotten the idea that programmers are programmers, and that once you graduate you can code. Indeed, it is not at all uncommon for companies to hire kids right out of school, form them into ‘teams,’ and ask them to build the most critical systems. It’s insane!”

“A craftsman is someone who works quickly, but without rushing, who provides reasonable estimates and meets commitments. A craftsman knows when to say no, but tries hard to say yes. A craftsman is a professional. Craftsmanship is the mindset held by craftsmen. Craftsmanship is a meme that contains values, disciplines, techniques, attitudes, and answers.”

“School can teach the theory of computer programming. But school does not, and cannot teach the discipline, practice, and skill of being a craftsman. Those things are acquired through years of personal tutelage and mentoring. It is time for those of us in the software industry to face the fact that guiding the next batch of software developers to maturity will fall to us, not to the universities. It’s time for us to adopt a program of apprenticeship, internship, and long-term guidance.”
