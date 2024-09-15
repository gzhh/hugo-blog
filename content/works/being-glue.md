---
title: "Being Glue"
date: 2024-08-18T16:38:52+08:00
lastmod: 2024-08-18T16:41:52+08:00
draft: false
categories: [Work]
tags: []
---


## 原文

https://noidea.dog/glue

As a professional software engineer，your task is to translate the English content I send you into Chinese.`

var content = `Your job title says "software engineer", but you seem to spend most of your time in meetings. You'd like to have time to code, but nobody else is onboarding the junior engineers, updating the roadmap, talking to the users, noticing the things that got dropped, asking questions on design documents, and making sure that everyone's going roughly in the same direction. If you stop doing those things, the team won't be as successful. But now someone's suggesting that you might be happier in a less technical role. If this describes you, congratulations: you're the glue. If it's not, have you thought about who is filling this role on your team?

Every senior person in an organisation should be aware of the less glamorous - and often less-promotable - work that needs to happen to make a team successful. Managed deliberately, glue work demonstrates and builds strong technical leadership skills. Left unconscious, it can be career limiting. It can push people into less technical roles and even out of the industry.

Let's talk about how to allocate glue work deliberately, frame it usefully and make sure that everyone is choosing a career path they actually want to be on.

Slides:
 You know that thing where everyone on a software engineering team turns up and just writes code for eight hours a day and then later the project is successful? No you don't. Projects don’t work like that!   Of course coding is an important skill in 
You know that thing where everyone on a software engineering team turns up and just writes code for eight hours a day and then later the project is successful? No you don't. Projects don’t work like that!

Of course coding is an important skill in a software engineering team. But there are a ton of other skills that we need to bring to work every day. Skills that can mean the difference between a project that succeeds and one that fails.

 Like noticing when other people in the team are blocked and helping them out. Or reviewing design documents and noticing what's being handwaved or what's inconsistent. Or onboarding the new people and making them productive faster. Or improving proc
Like noticing when other people in the team are blocked and helping them out. Or reviewing design documents and noticing what's being handwaved or what's inconsistent. Or onboarding the new people and making them productive faster. Or improving processes to make customers happy.

 I call all of this  glue work.   It's technical leadership, so we do get some signal for it on leadership interviews for senior engineers.  But sometimes a team ends up someone who  isn't  senior, but who happens to be good at this stuff. Someone wh
I call all of this glue work.

It's technical leadership, so we do get some signal for it on leadership interviews for senior engineers.

But sometimes a team ends up someone who isn't senior, but who happens to be good at this stuff. Someone who acts senior before they're senior. This kind of work makes the team better -- there's plenty of it to go around. But people aren't always rewarded for doing it.

 In fact, doing glue work too early can be  career limiting , or even push people out of the industry.  It's ironic. We lose good engineers because they happen to  also be good at other skills we need.  
In fact, doing glue work too early can be career limiting, or even push people out of the industry.

It's ironic. We lose good engineers because they happen to also be good at other skills we need.

 Hello, my name's Tanya and I'm a principal software engineer at Squarespace. I'm  whereistanya on twitter  and  github  and I blog here at  noidea.dog , which is of course a Squarespace site.  And today I want to talk about glue. 
Hello, my name's Tanya and I'm a principal software engineer at Squarespace. I'm whereistanya on twitter and github and I blog here at noidea.dog, which is of course a Squarespace site.

And today I want to talk about glue.

 Here's our agenda:  1) I'm going to tell a story of someone whose career is hurt by glue work. It's not exactly a true story but it's a lot of true stories combined. I've given this talk a few times now and I've had so many amazing emails from peopl
Here's our agenda:

1) I'm going to tell a story of someone whose career is hurt by glue work. It's not exactly a true story but it's a lot of true stories combined. I've given this talk a few times now and I've had so many amazing emails from people who’ve told me this was their story too.

2) Then we're going to talk about fairness, both in the outcome of the story and in how work is distributed

3) Then we're going to talk about whether and when to leave the IC engineer track and become a people manager, product manager, etc. I imagine a lot of people reading this have grappled with this decision at some point. There are a lot of opinions on how to make it and I'll give you one more :-)

4) Then how to frame your work if you've been doing a lot of glue, and how to makes your impact visible. Or how to help your coworkers or reports do the same thing.

5) And finally, I want to talk about learning and growing, which is something I don't think our industry talks about enough.

Ok, story time. Imagine a software engineer....

 Here she is, first day in a new team. She's been out of college a few years, had her first couple of tech jobs. She's not  wildly  confident in her skills, but likes the work. 
Here she is, first day in a new team. She's been out of college a few years, had her first couple of tech jobs. She's not wildly confident in her skills, but likes the work.

 The new codebase is very hairy and her first changes take a long time. This is normal, but everyone's busy with their own stuff and nobody's reassuring her. She feels like she's working too slowly, needing too much help. After a few weeks, she’s afr
The new codebase is very hairy and her first changes take a long time. This is normal, but everyone's busy with their own stuff and nobody's reassuring her. She feels like she's working too slowly, needing too much help. After a few weeks, she’s afraid they regret hiring her.  

 Then she gets her first win. A customer comes in with a request: they want data that the API really should be able to provide but the team hasn't prioritised this feature yet. Our friend here spends a couple of days manually getting the data the cus
Then she gets her first win. A customer comes in with a request: they want data that the API really should be able to provide but the team hasn't prioritised this feature yet. Our friend here spends a couple of days manually getting the data the customer needs. The customer is overjoyed.

She documents how to get that data, and some answers to other questions the team gets asked a lot on slack. The team stops getting so many interruptions. The customers are happy; the other software engineers are happy.

Ok, back to that difficult code.

 A while later, she gets talking with people on a nearby team. They seem to have a different idea of what problem they're all solving and they're going in a different direction. She sets up a meeting with System Designer on her team and asks a lot of
A while later, she gets talking with people on a nearby team. They seem to have a different idea of what problem they're all solving and they're going in a different direction. She sets up a meeting with System Designer on her team and asks a lot of questions. The thing changes direction. Now they're working together and building something better. She takes notes at the meeting and sends them around to make sure everyone has a shared understanding of what got agreed.

 New people join the team. She remembers her difficult first few weeks and writes a bunch of onboarding documents. She sets up a mentorship program, so all new people will get a mentor from now on. 
New people join the team. She remembers her difficult first few weeks and writes a bunch of onboarding documents. She sets up a mentorship program, so all new people will get a mentor from now on.

 The company keeps having outages, and they’re often attributed to lack of tests in the codebase. She gathers a bunch of senior people and keeps pushing until they agree on coding standards for the whole organisation. All code will be more tested, mo
The company keeps having outages, and they’re often attributed to lack of tests in the codebase. She gathers a bunch of senior people and keeps pushing until they agree on coding standards for the whole organisation. All code will be more tested, more readable and more reliable now. There are fewer rollbacks. Code review gets faster because the code's in a consistent style.

 The manager has a bunch of teams and is starting to rely on Engineer to know what's going on with this one. Hey, Awesome Coder seems blocked. Do you know what the deal is?  Our Engineer investigates, discovers that Awesome Coder needs information fr
The manager has a bunch of teams and is starting to rely on Engineer to know what's going on with this one. Hey, Awesome Coder seems blocked. Do you know what the deal is?

Our Engineer investigates, discovers that Awesome Coder needs information from another team but is mired in a three week long email thread. She talks to some people in real life, figures out what the confusion is, sorts it out. Awesome Coder is unblocked. He says thank you, writes thousands of lines of code. Since she now has a lot of state on the project, our Engineer writes his documentation and launch plan. The thing ships on time.

Well done Awesome Coder, says everyone.

Two years pass like this. Engineer keeps vowing that she will write more code soon….

 ….but every day, something more important comes up. They team has started to treat her as an unofficial lead. She has a broad view of everything that's going on and can spot the negative space between the designs and point out extra things that need
….but every day, something more important comes up. They team has started to treat her as an unofficial lead. She has a broad view of everything that's going on and can spot the negative space between the designs and point out extra things that need to happen. She has 1:1s with everyone. She's mentoring all the new people.

When she does have free time, it's an hour or two between meetings. The idea of swapping the code into her brain for two hours and then going to a meeting is really painful.

She's not worried though, because everyone tells her how great her work is. She always gets glowing performance reviews. In fact she feels like she's gone up a level.

 Let's see if her company's promotion process agrees. Who should we promote? 
Let's see if her company's promotion process agrees. Who should we promote?

 Well, obviously the person who wrote all that code! Well done Awesome Coder! 
Well, obviously the person who wrote all that code! Well done Awesome Coder!

 And the person who did the design for the thing, and made it integrate so well with the stuff they were building in the other team! Well done, System Designer!  Aaaaaand…. 
And the person who did the design for the thing, and made it integrate so well with the stuff they were building in the other team! Well done, System Designer!

Aaaaaand….

 …that’s it.  Wait, what? 
…that’s it.

Wait, what?

 Why not me? 
Why not me?

 Your project isn't finished. You're not producing much code. You didn't have enough impact yet. 
Your project isn't finished. You're not producing much code. You didn't have enough impact yet.

 But I decreased onboarding time. 
But I decreased onboarding time.

 I noticed that we were building the wrong thing and made us change course. 
I noticed that we were building the wrong thing and made us change course.

 Our customers say I'm the only person who helps them. 
Our customers say I'm the only person who helps them.

 I introduced coding standards and testing guidelines and now we have faster code reviews and fewer rollbacks. 
I introduced coding standards and testing guidelines and now we have faster code reviews and fewer rollbacks.

 I review all of our design documents and the comments I leave and the questions I ask make us build better things. 
I review all of our design documents and the comments I leave and the questions I ask make us build better things.

 They're like yes, this is good work. But you didn't really have a  technical  contribution. 
They're like yes, this is good work. But you didn't really have a technical contribution.

 “Wasn’t that technical? I mean, it wasn’t code, but not all technical work is code…” 
“Wasn’t that technical? I mean, it wasn’t code, but not all technical work is code…”

 And they say "Look, you're great at communication. Your soft skills are outstanding. We just don't think you're  an engineer . Consider becoming a project manager instead?" 
And they say "Look, you're great at communication. Your soft skills are outstanding. We just don't think you're an engineer. Consider becoming a project manager instead?"

 So was it fair? At every point, this engineer did the highest impact work that was available. The project wouldn't have shipped without her. She was the glue that held the whole thing together.  Over the last two years, she got really good at techni
So was it fair? At every point, this engineer did the highest impact work that was available. The project wouldn't have shipped without her. She was the glue that held the whole thing together.

Over the last two years, she got really good at technical leadership: understanding the problem domain, understanding the people, introducing standards, making the designs better. But she legitimately didn't any get better at coding.

What do we do with this? Is she a senior engineer?

 It’s ok if you feel conflicted here! I’ve asked this question a bunch of times now, and gotten responses right across the board. Several people have said that the answer is so obvious that the question didn’t need to be asked (though they don’t agre
It’s ok if you feel conflicted here! I’ve asked this question a bunch of times now, and gotten responses right across the board. Several people have said that the answer is so obvious that the question didn’t need to be asked (though they don’t agree on what the answer is), but most people have been conflicted about it.

One thing I'm certain of is that her manager bears some responsibility here. There was a communications breakdown.

 This shouldn't have been a surprise. This engineer got glowing performance reviews. She believed she was on the path to senior engineer. And you know, she did the kind of work and solved the kinds of problems I would expect senior or even staff engi
This shouldn't have been a surprise. This engineer got glowing performance reviews. She believed she was on the path to senior engineer. And you know, she did the kind of work and solved the kinds of problems I would expect senior or even staff engineers to pick up. She’s definitely got the leadership part covered.

But this company doesn't consider that to be sufficient promotable work, at least not at this level. Although they didn't explicitly say so, they want code or other quantifiable technical work. And her manager never told her she was doing too much non-promotable work. Probably the manager was probably just glad that the glue work was getting done. Because someone needed to do it.

 Glue work is the difference between a project that succeeds and one that fails. This is why technical program managers and project managers make such an impact: they do the ultimate glue role. They see the gaps and fill them.  In teams without a pro
Glue work is the difference between a project that succeeds and one that fails. This is why technical program managers and project managers make such an impact: they do the ultimate glue role. They see the gaps and fill them.

In teams without a project manager, what happens? In some teams, the manager takes up the load. In others, the work gets spread among the people willing to do it, or the people expected to volunteer for it.

 I read this  article about volunteering on hbr.org  (there's an  accompanying 35 page publication  if that’s more your speed). It showed that, when there is non-promotable work to be done, women volunteer to do it 48% more often than men.  But they 
I read this article about volunteering on hbr.org (there's an accompanying 35 page publication if that’s more your speed). It showed that, when there is non-promotable work to be done, women volunteer to do it 48% more often than men.

But they also found that men volunteered less because if they waited, they knew that a woman would volunteer. In all male groups, they had no trouble getting volunteers. If there were no women there, men volunteered just fine.

The even more interesting part was that, when managers were asked to choose someone to do thankless work, they asked women 44% more than they asked men.

(Article: https://hbr.org/2018/07/why-women-volunteer-for-tasks-that-dont-lead-to-promotions)

 I want to be clear that I'm not saying 100% of your work needs to be promotable work. It's good to build auxiliary skills and expand your horizons, and it's important for everyone to do their fair share of taking out the garbage. But a large percent
I want to be clear that I'm not saying 100% of your work needs to be promotable work. It's good to build auxiliary skills and expand your horizons, and it's important for everyone to do their fair share of taking out the garbage. But a large percentage of your work should be the thing you're evaluated on. If someone's doing very little of their core job, they are hurting their career. If you're their manager and letting them do that, you are letting them hurt their career.

Non-promotable work is one of those "one person's trash is another's treasure" things. Like, if an engineer organises an offsite, that's non-promotable work, but a people manager can maybe claim it's part of their job to do team-building. If an event coordinator does it, it's probably their core job.

Where there's work that is genuinely non-promotable for anyone, it needs to be shared. The work needs to be tracked whatever way the team tracks its other work, and it needs to be shared out deliberately. If it just gets done by whoever picks it up, it won't fall fairly.

I invite you take a moment to think about who's doing the non-promotable work on your team.

 Ok, back to our friend. Folks are now suggesting that she change to a role where that same work  would be  promotable. And I've seen this a lot of times: this message of "you're doing work that's not promotable, so change your  role ". I haven't see
Ok, back to our friend. Folks are now suggesting that she change to a role where that same work would be promotable. And I've seen this a lot of times: this message of "you're doing work that's not promotable, so change your role". I haven't seen as much emphasis on "so change your work" or indeed "so change how you tell the story of your work".

Let's talk about changing roles. I have read a lot of articles about deciding whether to do a role or not. Most of them are people who are doing a job talking about whether you're cut out to do that job. As if the ability to do something means that you have to do it.

They say: can you handle giving feedback, do you like coaching, do you like people? Then you should be a manager.

Can you put yourself inside the shoes of your customer? Then, yes, congratulations you're a product manager. IT IS DECIDED.

 But it's like those signs at carnivals: "You must be this tall to go on the rollercoaster". "Ok, I'm tall enough, but that looks horrible."  "You must be this socially competent to be a manager." I am, but that is not my idea of a good time!  I have
But it's like those signs at carnivals: "You must be this tall to go on the rollercoaster". "Ok, I'm tall enough, but that looks horrible."

"You must be this socially competent to be a manager." I am, but that is not my idea of a good time!

I have my own metric for it. If you code, you get better at coding. If you manage people, you get better at managing people.

So…

 ... what do you want to get better at?  What are the skills you want? It's not about what skills you already have! What skills do you wish you had? The vast majority of our learning happens on the job.  But I see people not considering the roles the
... what do you want to get better at?

What are the skills you want? It's not about what skills you already have! What skills do you wish you had? The vast majority of our learning happens on the job.

But I see people not considering the roles they want because they don't feel like they already have all the skills of that job. I've had a lot of CS college students tell me they're not applying for programming jobs because they don't feel like they're strong programmers.

Of course they aren't! They're still in college. The vast majority of our learning happens on the job.

 They end up choosing a role that they don't want because they're scared of doing the role they do want, or because someone else tells them that they  would be good at  the other role.  I advise people to choose deliberately. Choose a role that you'l
They end up choosing a role that they don't want because they're scared of doing the role they do want, or because someone else tells them that they would be good at the other role.

I advise people to choose deliberately. Choose a role that you'll feel successful and happy and proud to say you do, and that will teach you skills you want. Do a job you’re excited by. You will learn to get good at it by doing it. I feel like we don't admit it often enough enough that most of the time, we won't do a job well on day one. The vast majority of our learning happens on the job.

There's another consideration though, especially when people make this decision in college or when they're junior. Taking a step away from a more technical role closes doors. It's not fair, but our industry biases are set up so that you really need to have a solid engineering resume before you take a non-engineering role.

 Because the moment you give up that engineer title, the moment the most recent job on LinkedIn does not have the word "engineer" in it, half the industry will assume your existing tech skills are *gone forever* and you're somehow incapable of acquir
Because the moment you give up that engineer title, the moment the most recent job on LinkedIn does not have the word "engineer" in it, half the industry will assume your existing tech skills are *gone forever* and you're somehow incapable of acquiring more. I don't know how this brain science is supposed to work, but it's such a common implicit bias.

Especially if your job title is any variant on "project manager". Many people will immediately assume you are not good at technology.

Kripa Krishnan, the legendary director of cloud product operations at Google once said that while she'd experienced some industry prejudice for being female and some for having an accent, it was nothing compared to the prejudice she experienced for being a TPM.

Project managers and TPMs are routinely underestimated by engineers.

 I have seen a lot of people take a role like this and, find themselves pushed towards being a non-technical manager or project manager: a step towards leaving the industry. I've seen some look back at engineer jobs and discover that they also can't 
I have seen a lot of people take a role like this and, find themselves pushed towards being a non-technical manager or project manager: a step towards leaving the industry. I've seen some look back at engineer jobs and discover that they also can't get hired at the level of developer they used to be, even if it was quite recent. As if the skills they had have evaporated.

They have to come in at a lower level than they left because people don’t believe they are capable of the job. They will invariably hear the three most infuriating words in the industry:

 “NOT TECHNICAL ENOUGH". What even is this. What is "technical" here? How do you do anything actionable with that? It's so domain-specific!  If you're ever tempted to tell someone they're not technical enough, well, first of all just don’t. But be re
“NOT TECHNICAL ENOUGH". What even is this. What is "technical" here? How do you do anything actionable with that? It's so domain-specific!

If you're ever tempted to tell someone they're not technical enough, well, first of all just don’t. But be really specific about what you need them to know. Like:

"You need to understand and participate in the technical discussion in design meetings, so please get comfortable with the tradeoffs in this set of technology. Here's a book I recommend.

Or: "Our senior engineers are all system designers. Please take some distributed systems classes and have opinions about the CAP theorem.

Otherwise, you're basically only saying…

 " You don't really seem like an engineer.  Could you be more engineer-y?"  It's gatekeeping. It's not actionable feedback.  Which brings us back to our friend. 
"You don't really seem like an engineer. Could you be more engineer-y?"

It's gatekeeping. It's not actionable feedback.

Which brings us back to our friend.

 Two years ago she joined as a mid-level engineer. Since then she's spent her time filling gaps to make the team and the organisation succeed. As a result, she's just been told she doesn't have technical accomplishments.  And she'd like a promotion. 
Two years ago she joined as a mid-level engineer. Since then she's spent her time filling gaps to make the team and the organisation succeed. As a result, she's just been told she doesn't have technical accomplishments.

And she'd like a promotion. Let's talk about that for a second. I'm putting a lot of emphasis on career advancement and that's not a priority for everyone. That's fine. Here's an explicit bias I have: I want this engineer lady to feel fulfilled and also to have long-term financial security. She'd like to some day retire and buy a little boat. I want to help with that.

I don’t know what her right career choice is. Only she can make that decision. She should choose deliberately, based on:

* what would she love to get better at?

* What is a job that she'll feel happy and proud to do?

* What doors is she comfortable making hard to reopen?

and unfortunately one more:

 * Where will she feel safe?  If she chooses a role she’s less excited about but where she feels more supported and less alone, I can't judge her for that. But I hope she gets to do something she loves. 
* Where will she feel safe?

If she chooses a role she’s less excited about but where she feels more supported and less alone, I can't judge her for that. But I hope she gets to do something she loves.

 Either way, I will respect her decision :-)  But, because I don’t have time to do Choose Your Own Adventure in slides, for the rest of this talk we're going to assume she decides to stay as an engineer, because I think we don't talk about the senior
Either way, I will respect her decision :-)

But, because I don’t have time to do Choose Your Own Adventure in slides, for the rest of this talk we're going to assume she decides to stay as an engineer, because I think we don't talk about the senior IC path enough and I'm a fan.

So, she wants to be a senior engineer and, really, she's already doing most of that job. But she's getting a whole lot of "not technical enough".

What do you do if you’re glue? What do you do if you're managing someone who's glue? How do you make sure not to waste this valuable skillset? Here's a four step plan.

 First off, there needs to be a long-overdue career conversation between this engineer and her manager. She needs to ask direct questions like "Will I get promoted next round?" "What work do I need to do to get promoted?" "Is this senior engineer wor
First off, there needs to be a long-overdue career conversation between this engineer and her manager. She needs to ask direct questions like "Will I get promoted next round?" "What work do I need to do to get promoted?" "Is this senior engineer work?”

Her manager needs to be honest and direct too, based on their understanding of the career ladder.

They need to agree on goals, make a plan, and check in at intervals and make sure they're still on track.

 Second: a job title. If she and her manager want her to continue doing a lot of glue work, is there a title that gives her tech credibility? Can she become technical lead or something? People expect a lead to do a ton of glue.  Ok, there are probabl
Second: a job title. If she and her manager want her to continue doing a lot of glue work, is there a title that gives her tech credibility? Can she become technical lead or something? People expect a lead to do a ton of glue.

Ok, there are probably some people reading this and thinking titles don't matter. And maybe you don't need them, internet person! But that doesn't mean other people don't.

Look. If you're a white or Asian dude, everyone assumes you're good at coding, just by default. You could have graduated yesterday with a degree in law, and people assume you can code.

For the rest of us, we don't get that free assumption. A job title saves time and energy that we don’t need to spend putting our credentials on the table. It gives us some hours back in our week.

And it gives us some freedom to do glue work without people deciding we're "not technical enough" any more.

If you're telling underrepresented folks in your org that titles don't matter, you're doing them a disservice. Titles matter a ton.

 Third, she needs artifacts of her work that show her impact and tell the story. Due to her work and her technical judgement, this thing happened.  Her manager should be telling the same story. If you see this situation, where a glue person is the on
Third, she needs artifacts of her work that show her impact and tell the story. Due to her work and her technical judgement, this thing happened.

Her manager should be telling the same story. If you see this situation, where a glue person is the only reason something launched, publicly give them credit! And not for helping, but for leading.

She should be creating and saving artifacts that back up this narrative: design proposals, meeting notes, group emails, crucial points where she made the thing happen.

Now, this might still not work. It might be six months later, and the promotion folks say no again. In that case, I have a solution that's a bit cynical. If you’re not getting promoted for glue work…

 .stop doing glue work. I would advise her to -- temporarily -- do EXACTLY the thing on the job ladder, even if it means letting more important things drop.  She should do some easily quantifiable technical work. Write a bunch of code. Write some des
.stop doing glue work. I would advise her to -- temporarily -- do EXACTLY the thing on the job ladder, even if it means letting more important things drop.

She should do some easily quantifiable technical work. Write a bunch of code. Write some designs that anyone could have written. Learn how to performance tune the database. Do something that is unarguably "technical".

She should do it even if she's not the best person on the team to do it. Even if she's rusty and she'll be a lot slower than someone else.

But the thing is, that means she has to stop doing the other stuff.

 Coding can't fit around a full calendar of glue work.  So I'd advise her, until the promotion's through, to declare a lot of things not her problem. 
Coding can't fit around a full calendar of glue work.

So I'd advise her, until the promotion's through, to declare a lot of things not her problem.

 Stop interviewing, stop organising the off-sites, stop onboarding, stop fielding requests from users, stop anything that sounds like team building. Stop helping other people with their work. Archive mail. Quit slack channels. Do not curate the team 
Stop interviewing, stop organising the off-sites, stop onboarding, stop fielding requests from users, stop anything that sounds like team building. Stop helping other people with their work. Archive mail. Quit slack channels. Do not curate the team roadmap.

Crucially: don't catch things that are about to drop. That's incredibly hard for a lot of us, but remember that the rest of the team already does this.

Stop being the unofficial lead. (If you're in the same situation and you're the official lead, consider stopping that too!)

And… oof, I hate saying this, and please forgive me, but if she does a lot of diversity work for her company, I would recommend she stop doing diversity work for a while.

  Getting promoted is diversity work.  Being visibly successful is the most powerful diversity work she can do. She can be the representation someone needs. She can be in a much better position for mentorship and sponsorship.  But she needs free time
Getting promoted is diversity work. Being visibly successful is the most powerful diversity work she can do. She can be the representation someone needs. She can be in a much better position for mentorship and sponsorship.

But she needs free time in her calendar to get there.

 Only if you make a lot of things not your problem can you go from this…  
Only if you make a lot of things not your problem can you go from this…

 …to this. (And ideally better than this, but this is the best I could make my calendar look to do a screenshot :-))  Those big empty spaces are good to block out for project work, coding, writing designs and so on. And that work will have a side eff
…to this. (And ideally better than this, but this is the best I could make my calendar look to do a screenshot :-))

Those big empty spaces are good to block out for project work, coding, writing designs and so on. And that work will have a side effect, it's a virtuous cycle, which is that the person doing it will get better at coding and writing designs.

 The technical term for this is *learning* :-D  The vast majority of our learning happens on the job.   If the skills you wish you had are part of the job you're doing all day, you get a certain amount of learning for free. Every time you hit stack o
The technical term for this is *learning* :-D The vast majority of our learning happens on the job.

If the skills you wish you had are part of the job you're doing all day, you get a certain amount of learning for free. Every time you hit stack overflow, you're learning something. But for anything that you're not repeatedly doing, you have to go out and choose to learn it.

 Even for people who are getting recognised for glue work and who want to keep doing it, I really recommend you keep increasing your other skills.   If you only do glue, you will only get better at glue.  You're making your team more effective but yo
Even for people who are getting recognised for glue work and who want to keep doing it, I really recommend you keep increasing your other skills.

If you only do glue, you will only get better at glue. You're making your team more effective but you're hurting your future self. No matter what you end up doing, you are unlikely to regret feeling more confident in core technical skills.

Learning is something our industry doesn't talk about a ton. All of the tech knowledge that is in people's brains, they've learned in some way. But we don’t really admit that it took time.

 If you're a senior person, please, show the junior people in your organisation that you're learning and how you're doing it. Be public about what you're learning.  Some of us have the amazing privilege of having free time to learn. Others have oblig
If you're a senior person, please, show the junior people in your organisation that you're learning and how you're doing it. Be public about what you're learning.

Some of us have the amazing privilege of having free time to learn. Others have obligations that mean they have literally zero free time.

So make it clear that it's okay -- and normal -- to learn at work, during work hours.

Turning your mid-level people into senior people is a good investment. Never waste an opportunity to have people learn things.

 Even more than that, watch out for learning opportunities that you're wasting. If you're sheltering someone by always doing something for them, you're depriving them of a learning opportunity.  If you have a thing you always do, that you know how to
Even more than that, watch out for learning opportunities that you're wasting. If you're sheltering someone by always doing something for them, you're depriving them of a learning opportunity.

If you have a thing you always do, that you know how to do, find someone who would benefit from learning it, and ask them -- nicely! -- if they'd like to take it over. Then get them to block out time on their calendar to learn about it, and give them your support as they do.

We all only get better at what we spend time on. And we do get better if we spend time on things.

And not just technical things.

 My amazing colleague Polina has advice on what she says when someone tries to push her into more humaning work than is good for her. They say "but you should do it because you're so good at communication." She says "Yes, I'm good at everything I put
My amazing colleague Polina has advice on what she says when someone tries to push her into more humaning work than is good for her. They say "but you should do it because you're so good at communication." She says "Yes, I'm good at everything I put effort into. You should see me doing systems design."

So, while she's off designing systems, she's giving other people on the team a learning opportunity to become good at communication too, by putting effort into it.

If you're a manager, I encourage you to help the non-glue people on your team also put effort into communication. Remember these two dudes in the story at the start?

 The Awesome Coder only succeeded because someone else on the team went and talked to other people and broke him out of the email thread of doom. He couldn't communicate well enough to ask another team for some data that he needed.  The System Design
The Awesome Coder only succeeded because someone else on the team went and talked to other people and broke him out of the email thread of doom. He couldn't communicate well enough to ask another team for some data that he needed.

The System Designer only succeeded because someone else on the team asked what the thing he was building was actually for. He didn't have the technical judgement to step back and understand how his system would integrate with the other systems the company was building and to be clear about the problem they were all solving.

Should they have been promoted? Are they really senior engineers? I don't think they are. And they won't learn to be if people keep doing their glue for them. They will get better at what they spend time on. The vast majority of their learning will happen on the job.

Managers: If your job ladder doesn't require that your senior people have glue work skills, think about how you're expecting that work to get done.

Glue people: push back on requests to do more than your fair share of non-promotable work and put your effort into something you want to get good at.

 Our skills aren't fixed in place.  You can be good at lots of things.  You can do anything. 
Our skills aren't fixed in place. You can be good at lots of things. You can do anything.


## OpenAI 翻译的译文

你的职位是“软件工程师”，但你似乎大部分时间都在开会。你希望有时间去编码，但没有其他人负责新工程师的培训、更新路线图、与用户沟通、注意到被忽略的问题、咨询设计文档中的问题，并确保每个人都在大致相同的方向上。如果你停止做这些事情，团队就不会那么成功。但是现在有人建议你可能在一个技术性较低的角色中会更快乐。如果这描述的是你，那么恭喜你：你就是“胶水”。如果不是，你有没有想过谁在你的团队中担任这个角色？

组织中每一个高级人员都应该意识到那些不那么光鲜亮丽的——通常也不太容易晋升的——工作对团队成功的重要性。 如果管理得当，胶水工作能够展现并培养强大的技术领导能力。如果心中无意识，它可能会限制职业发展，甚至使人离开这个行业。

让我们来谈谈如何有意识地分配胶水工作，合理框架并确保每个人都选择自己真正想走的职业道路。

幻灯片：
你知道那种情况吗？每个软件工程团队的成员都到齐，整整八小时只写代码，最后项目成功？不，你不知道。项目并不是这样运作的！当然，编码是软件工程团队中的一项重要技能。但是，我们每天需要带来大量其他技能。这些技能可能决定一个项目是否成功。

比如，注意团队中其他人是否被阻塞并帮助他们；或者审查设计文档，发现哪些地方是草草了事，哪些地方是不一致的；又或者进行新员工培训，让他们更快地投入工作；或者改善流程来让客户满意。

我将这一切称为胶水工作。这是技术领导力，因此在高级工程师的领导面试中，我们确实会收到一些信号。但有时候，一个团队中会出现一些并不高级，但正好擅长这项工作的成员。某种程度上，他们在还没晋升之前就表现得像个高级工程师。这种工作使团队变得更好——而且有很多这样工作可以进行。但是，人们并不总是因为这样做而得到奖励。

事实上，过早从事胶水工作可能会限制职业发展，甚至使人离开这个行业。这很讽刺。我们失去了优秀的工程师，因为他们恰巧也在其他我们需要的技能上表现出色。

大家好，我是塔尼亚，我是一名 Squarespace 的首席软件工程师。我在 Twitter 和 GitHub 上是 whereistanya，并在这里工作：noidea.dog，当然这是一个 Squarespace 网站。今天我想谈谈胶水工作。

这是我们的议程：
1) 我将讲述一个因为胶水工作而受损的人的故事。这不完全是一个真实的故事，但它结合了很多真实故事。我已经做过几次这个演讲，收到了许多来自告诉我“这也是我的故事”的人的精彩邮件。

2) 然后我们将谈论公平性，包括故事的结果和工作分配的方式。

3) 接下来，我们会讨论是否以及何时离开 IC 工程师轨道，成为人员经理、产品经理等。我想很多读者在某个时候都为此苦恼过。关于如何做出这个决定有很多看法，我会再给你一个 :-)

4) 如何框架你的工作，如果你做了很多胶水工作，就如何让你的影响可见。或者如何帮助你的同事或下属做到这一点。

5) 最后，我想谈谈学习和成长，这是我认为我们行业讨论得不够多的事情。

好了，故事时间。想象一个软件工程师...

她在新团队的第一天。她已经大学毕业几年，经历了第一两个技术工作。她的技能信心不是特别强，但很喜欢这个工作。

新的代码库很复杂，她的第一次修改花了很多时间。这很正常，但每个人都忙于自己的事情，没有人安慰她。她觉得自己工作得太慢，需要太多的帮助。几周后，她担心他们会后悔雇用她。

然后她得到了第一次胜利。一个客户提出请求：他们想要 API 应该提供的数据，但团队尚未优先考虑这个功能。我们的朋友花了几天时间手动获取客户所需的数据。客户感到非常高兴。

她记录了如何获取这些数据，并解决了一些团队在 Slack 上经常被问到的问题。团队不再受到这么多干扰。客户很高兴，其他工程师也很高兴。

好吧，回到那段困难的代码。

过了一段时间，她与一个邻近团队的人交谈。他们似乎对自己所解决的问题有不同的看法，并且在不同的方向上。她与团队中的系统设计师安排了一次会议并提出了很多问题。事情开始向着新的方向发展。现在他们在一起合作，构建更好的东西。她在会议上做了笔记，并发送给大家，以确保每个人都对达成的共识有共同的理解。

新员工加入团队。她记得自己经历过的艰难前几周，便撰写了一些新员工培训文档。她设立了一个导师计划，从现在开始所有新员工都会有一个导师。

公司不断遭遇故障，通常被归因于代码库中缺少测试。她召集了一些高级人员，继续推动他们就整个组织达成编码标准。所有代码将更加测试、可读和可靠。回滚次数减少。由于代码风格一致，代码审查变得更快。

经理有几个团队，开始依赖她来了解这个团队发生的事情。“嘿，优秀的编码者似乎被阻塞了。你知道发生了什么吗？”我们的工程师进行了调查，发现优秀的编码者需要来自另一个团队的信息，但正在被一个长达三周的电子邮件线程缠住。她与一些人面对面交谈，弄清了混乱所在，并解决了它。优秀的编码者得以恢复工作。他表示感谢，写下数千行代码。由于她现在对项目有了很多了解，我们的工程师撰写了他的文档和发布计划。事情按时发布了。

“做得好，优秀的编码者，”大家说。

两年就这样过去了。工程师继续宣誓她很快会写更多的代码…

…但每天都会出现更重要的事情。团队开始把她视为一个非正式的领导。她对正在发生的事情有广泛的了解，可以发现设计之间的负空间，并指出需要发生额外的事情。她与每个人进行一对一的沟通。她正在指导所有的新员工。

当她确实有空闲时间时，是在两次会议之间的一个小时或两个小时。将代码填充到她的脑海中两个小时，然后再去开会的想法让她感到非常痛苦。

尽管如此，她并不担心，因为每个人都告诉她她的工作有多好。她总能收到良好的绩效评估。事实上，她觉得自己已经提升了一个等级。

让我们看看她公司的晋升过程是否也这么认为。我们应该晋升谁呢？

好吧，显然是那个写了所有代码的人！做得好，优秀的编码者！

还有那个设计这个东西的人，并且让它与他们在其他团队构建的东西完美集成的人！做得好，系统设计师！

然后……

就是这样。等一下，什么？

“为什么不是我？”

“你的项目尚未完成。你没有产生足够的代码。你还没有产生足够的影响。”

“但我减少了入职时间。”

“我注意到我们在构建错误的东西，并让我们改变了方向。”

“我们的客户说我是唯一能帮助他们的人。”

“我介绍了编码标准和测试指南，现在我们的代码审查更快，回滚次数更少。”

“我审查了所有的设计文档，我留下的评论和提问使我们构建更好的东西。”

他们说：“是的，这很好。但你实际上没有什么技术贡献。”

“那不是技术性的吗？我的意思是，这不是代码，但并不是说所有的技术工作都是代码……”

“看，你很擅长沟通。你的软技能很出色。我们只是觉得你不是工程师。考虑一下成为项目经理吧？”

那么这是公平的吗？在每一个节点上，这位工程师都做了可用的最大影响工作。没有她，项目就不会发货。她是让整个事情得以运转的“胶水”。

在过去两年中，她在技术领导上变得非常出色：理解问题领域、理解人群、引入标准、改善设计。但她确实没有在编码上有所提高。

我们该怎么做？她算不算一名高级工程师？

如果你对此感到矛盾，那是可以理解的！我已经问过这个问题无数次，并得到了各式各样的回应。几个人表示答案显而易见，不用问（尽管他们对答案不一致），但大多数人对此感到矛盾。

我确信的一件事是，她的经理对此负有一定责任。沟通出现了问题。

这不应该是个惊喜。这位工程师得到了良好的绩效评估。她相信自己在向高级工程师的道路前进。你知道，她做了我认为高级工程师或工作人员工程师应承担的那类工作。她显然在领导方面做得很好。

但这家公司认为这不足以晋升，至少在这个级别上。虽然他们没有明确说明，但他们想要代码或其他可量化的技术性工作。而她的经理从未告诉她她在做过多不可晋升的工作。可能经理只是高兴胶水工作被完成了。因为总得有人来做。

胶水工作是决定一个项目成功和失败之间的差别。这就是技术项目经理和项目经理为何影响如此之大的原因：他们担任最终的胶水角色。他们发现缺口并填补它们。

在没有项目经理的团队中，会发生什么呢？在某些团队中，经理承担了这个负担。在其他团队中，这项工作被分配给那些愿意做的人，或者是那些被期望自愿做的人。

我在 hbr.org 上读了一篇关于志愿者的文章（如果你喜欢，可以查阅那篇附带的 35 页出版物）。它显示，当有非晋升性工作要做时，女性志愿者的比例比男性高 48%。但他们还发现，男性志愿者的比例较低，因为如果他们等待，他们知道一名女性会主动报名。在全部男性小组中，他们毫不费力地就能征得志愿者。如果没有女性在场，男性会自愿参与。

更有趣的是，当经理被要求选择一个人来做乏味的工作时，他们选女性的频率比男性高 44%。

（文章：https://hbr.org/2018/07/why-women-volunteer-for-tasks-that-dont-lead-to-promotions）

我想明确的是，我并不是说你100%的工作都需要是可晋升的工作。建立辅助技能、拓展视野是好的，每个人都应公平地分担一些琐碎的工作。但是，你的工作中很大一部分应该是你被评估的内容。如果某人几乎不做核心工作，他们会损害自己的职业发展。如果你是他们的经理而让他们这样做，你正在纵容他们的职业生涯受损。

不可晋升的工作就像“一个人的垃圾是另一个人的宝藏”。例如，如果一名工程师组织一个外出活动，那是不可晋升的工作，但人员经理可能会认为这是他们的团队建设工作。如果一名活动协调员做这件事，那就可能是他们的核心工作。

在任何不仅限于个别人的"非晋升性"工作出现的情况下，这项工作需要被分享。工作需要以团队追踪其他工作的方式进行追踪，并且需要有意地进行分配。如果工作只是由那些愿意承担的人去完成，就不会公平地分配。

我邀请你花一点时间思考一下，谁在你的团队中承担了这项不可晋升的工作。

好吧，回到我们的朋友。人们现在建议她变换角色，在那个角色中，胶水工作将变得可晋升。我看到这个情况很多次：“你在做不可晋升的工作，所以换个角色”。我没有看到太多强调“所以改变你的工作”或者“所以改变你叙述工作的方式”。

让我们谈谈转角色。我读过很多关于决定是否进行某个角色的文章。多数文章是做某种工作的人谈论你是否适合做这项工作。就好像做某事的能力意味着你必须去做。

他们会说：你能接受反馈，喜欢指导别人，喜欢人吗？那么你应该成为经理。

你能设身处地为客户着想吗？那么，是的，恭喜你，你是一个产品经理。决定了。

但这就像游乐场的标志：“你必须这么高才能玩过山车”。“好吧，我足够高，但这看起来太可怕了。”“你必须这么社交能力强才能成为经理。”我可以，但我并不想做那样的工作！

我有自己的标准。如果你编码，你就会在编码方面变得更好。如果你管理人员，你就会在人员管理方面变得更好。

那么……

……你想在哪方面变得更好？你想要什么技能？这不是关于你已经拥有的技能！你希望拥有什么技能？我们的大部分学习发生在工作中。但是我看到人们没有考虑他们想要的角色，因为他们觉得自己并没有所有那些工作的技能。我听过很多计算机科学学院的学生告诉我，他们没有申请编程工作，因为他们觉得自己不是强大的程序员。

当然，他们不是！他们还在大学。大部分学习发生在工作中。

他们最终选择了自己不想要的角色，因为他们害怕担任自己想要的角色，或者因为其他人告诉他们，他们会很适合另一个角色。我建议人们要有意识地选择。选择一个你会感到成功、快乐和自豪的角色，并能帮助你提高想要的技能。做一份你热衷的工作。你会通过实践变得更加出色。我觉得我们很少承认，我们很多时候在第一天并不会很好地完成一项工作。我们的大部分学习就在工作中发生。

不过，还有另外一个考虑，尤其是当人们在大学或刚入行时做出这个决定时。远离技术角色会关闭一些门。这不公平，但我们的行业偏见设置，使得你确实需要有一个扎实的工程简历才能接受非工程角色。

因为当你放弃工程师职位的那一刻，当你在 LinkedIn 上最近的职位中没有“工程师”一词时，行业的一半会认为你原有的技术技能*永远消失*，而你似乎无法获得更多。我不知道这种大脑科学如何运作，但这是一个如此常见的隐性偏见。

尤其是如果你的职称是任何变体的“项目经理”。很多人会立即假定你不太精通技术。

谷歌著名的云产品运营总监 Kripa Krishnan 曾说过，尽管她经历过一些行业对女性和口音的偏见，但与她作为技术项目经理的角色相比，这些都不算什么。

工程师们常常低估项目经理和技术项目经理。

我见过很多人接受这样的角色，发现自己被推向非技术性经理或项目经理的方向：这是一步退出行业的旅程。我见过一些人回头看工程师的工作，发现他们也不能以他们以前的开发人员水平被雇佣，即便这很近。就好像他们的技能消失了。

他们不得不从比离开时更低的级别入职，因为人们不相信他们能胜任这份工作。他们不可避免地会听到行业中最令人恼火的三个字：

“技术能力不足”。这甚至是什么？在这里“技术”是什么？你怎么能就此采取任何可行的行动？这太特定于领域了！

如果你曾经想告诉某人他们技术能力不足，嗯，首先，别这样。但请具体说明你希望他们了解的东西。比如：

“你需要在设计会议中理解并参与技术讨论，因此请确保熟悉这套技术的权衡。以下是我推荐的书。”

或者：“我们的高级工程师都是系统设计师。请上几节分布式系统课程，并对 CAP 理论形成看法。”

否则，你基本上只是在说：

“你似乎不太像工程师。能不能更像工程师一点？”这是设置障碍，不是可操作的反馈。这把我们带回到我们的朋友身上。

两年前，她以中级工程师的身份加入。此后，她花时间填补空白，使团队和组织的成功成为可能。因此，她刚刚被告知自己没有任何技术成就。

而她希望获得晋升。让我们谈谈这个。我在职业发展上强调了很多，这并不是每个人的优先事项。这没关系。这里有一个明确的偏见：我希望这位女工程师感到满足，并且有长期的财务安全。她希望某天能够退休并买一条小船。我想帮忙实现这个。

我不知道她的正确职业选择是什么。只有她才能做出这个决定。她应该有意识地选择，基于：

* 她希望在哪些方面得到提升？

* 哪份工作会让她感到快乐和自豪？

* 她在哪些方面会感到舒适，哪怕这意味着关闭一些大门？

还有不幸的一点：

* 她会在什么地方感到安全？如果她选择一份让她不那么兴奋但又感到更有支持的角色，我无法对此进行评判。但我希望她能够做一些她热爱的事情。

无论如何，我会尊重她的决定 :-) 但是，由于我没有时间在幻灯片上开展“选择你自己的冒险”，接下来的演讲我们将假设她决定继续作为工程师，因为我认为我们没有足够讨论高级 IC 的路径，我对此很感兴趣。

所以，她想成为一名高级工程师，实际上，她已经在做这份工作的大部分内容。但她却收到了很多“技术能力不足”的反馈。

如果你是胶水，应该怎么做？如果你在管理一个胶水工程师，应该怎么做？如何确保不浪费这个宝贵的技能集？下面是一个四步计划。

首先，这名工程师和她的经理之间需要进行一次长久未谈的职业对话。她需要问一些直接的问题，比如：“我下次被晋升吗？” “我需要做些什么工作才能获得晋升？” “这是高级工程师的工作吗？”

她的经理也需要根据对职业晋升路径的理解保持诚实和直接。

他们需要达成共识，设定目标，制定计划，并定期检查，确保仍然在正确的轨道上。

第二：一个职称。如果她和她的经理希望她继续从事大量胶水工作，是否存在某个职位可以赋予她技术上的可信度？她能否成为技术领导或者其他职称？人们期望领导会做大量胶水工作。

好吧，可能有些读者会觉得职称无所谓。也许你不需要它，但这并不意味着其他人不需要。

看看。如果你是白人或亚裔男性，大家默认你很擅长编码。即使你昨天才毕业，获得的是法律学位，大家依然会假设你能够编码。

对其他人来说，我们无法获得这种默认的假设。职称节省了我们不需要花时间与精力来铺陈资历，它让我们能找回一些工作时间。

它也给我们一些空间去做胶水工作，而不是人们再决定我们是否“技术能力不足”。

如果你在组织中对边缘化人群说“职称无所谓”，你扭曲了他们的职业发展。

第三，她需要能展示她影响力并讲述故事的工作成果。由于她的工作和技术判断，这件事发生了。她的经理也应该讲述同样的故事。如果你看到这种情况，有人是胶水工作导致某事启动，公开给他们信用！而且不仅仅是帮助，而是为了引导。

她应该创建和保存能够支持这一叙述的成果物：设计提案、会议记录、邮件群发、关键时刻的记录等，她使事情得以发生。

现在，这可能仍然行不通。可能六个月后，晋升委员会又一次拒绝。在这种情况下，我有一个略显悲观的解决方案。如果你因胶水工作没有获得晋升……

……停止进行胶水工作。我会建议她——暂时——去做工作梯度上正好的事情，即使这意味着让更重要的事情下滑。她应该做一些轻松可量化的技术工作。写很多代码。写一些任何人都可以评论的设计。学习如何优化数据库性能。做一些毫无争议的“技术工作”。

她应该这样做，即使她不是团队中最适合去做这件事的人。即使她有点生疏，速度比其他人慢得多。

但事情是，这意味着她必须停止其他的工作。

编码工作无法在满日程的胶水工作周围进行协调。所以我会建议她，在晋升之前，宣告很多事情不是她的问题。

停止面试，停止组织外出活动，停止新员工培训，停止回应来自用户的请求，停止所有听起来像团队建设的活动。停止帮其他人完成他们的工作。归档邮件，退出 Slack 频道。不要策划团队路线图。

最重要的是：不要去捕捉即将掉落的事情。这对我们中的很多人来说都是极其困难的，但请记住，其他团队成员会这样做。

停止成为非官方的领导。如果你处于同样的境地且你是正式的领导，考虑停止这一角色！

我非常不喜欢这样说，请原谅我，但如果她为公司做了很多多样性工作，我建议她暂时停止这项工作。

获得晋升就是多样性工作。明确地获得成功是她可以做到的最有力的多样性工作。她可以成为某人的代表。她可以处于更好的位置进行指导和支持。但她需要在日历中留出空闲时间才能做到这一点。

只有当你把很多事情当作不是你的问题时，你才能从这个……

……转换为这个。（理想情况下，比这个更好，但这是我能够做到的最好的日历截图:-) 这些大空白区域非常适合用于项目工作、编码、编写设计等等。而且，这项工作会产生一个副作用，这是一个良性循环，那就是从事这些工作的人将变得更优秀。

这个技术术语是*学习* :-D 我们的大部分学习都发生在工作中。如果你希望掌握的技能是你日常工作的内容，你能自然而然地获得一部分学习。每当你去 Stack Overflow 探索时，实际上你也在学习。但对于那些你没有反复做的事情，你需要出来选择学习。

即使是那些获得胶水工作认可而希望继续做这项工作的人，我真的建议你继续提升你的其他技能。

如果你只做胶水工作，你只会在胶水工作方面变得更好。你让团队变得更有效，但也在伤害着未来的自己。无论你最终会做什么，你都不太可能会后悔提升核心技术技能的自信。

学习是我们这个行业不太常谈论的事情。所有这些技术知识，都是以某种方式学来的。但我们并没有承认这需要时间。

如果你是一个高级人员，请向你组织中的初级人员展示你在学习，并且你是如何做到这一点的。公开分享你正在学习的内容。

我们中有一些人有时间去学习。其他人则因为需要满足某些责任而几乎没有空闲时间。

所以让大家明白，在工作中、工作时间内学习是正常的也是可以接受的。

将中级员工培养成高级员工是个很好的投资。永远不要错过让人们学习的机会。

更进一步，注意你浪费的学习机会。如果你通过总是替别人做事来保护某个人，你也在剥夺他们学习的机会。如果你有一项你一直做到的工作，并且你知道怎么做，找一个能从中受益的人，礼貌地询问他们是否想接手这项工作。然后让他们在日历中安排时间来学习，并在他们学习的过程中给予他们你的支持。

我们所有人只会在我们花时间的事情上变得更好。只要我们把时间花在某些事情上，我们就能更出色。

不仅仅是技术性的事情。

我出色的同事波琳娜给出了一些建议，她是如何应对那些试图让她承担超过她应有的人际工作的人。他们会说：“但你应该负责，因为你如此擅长沟通。”她会说：“是的，我在投入努力的一切事情上都很出色。你应该看看我做系统设计时的表现。”

因此，在她进行系统设计的同时，她还为团队中的其他成员提供了一个学习机会，让他们努力学习如何进行沟通。

如果你是经理，我鼓励你帮助团队中的非胶水成员，也努力提升沟通能力。记住故事开始时的那两个家伙吗？

优秀的编码者之所以取得成功，是因为团队中的其他人跑去与其他人沟通，并使他摆脱了那条无休止的邮件线程。他无法有效地与其他团队沟通，以获取他所需的一些数据。

系统设计师之所以成功，是因为团队中的其他人询问他正在构建的内容究竟是为了什么。他没有技术判断力去后退一步理解他的系统将如何与公司正在构建的其他系统集成，并清楚厘清他们要解决的问题。

他们是否应该被晋升？他们真的算高级工程师吗？我认为他们不算。如果人们继续为他们做胶水工作，他们将无法学会。随着时间的推移，他们只会变得更加出色。他们的学习将在工作中产生。

经理们：如果你的职业进阶路径没有要求高级人员具备胶水工作技能，请考虑你期待这项工作的完成方式。

胶水工作者：抵制要求你进行过多非晋升工作的请求，把你的精力投入到你想精通的事情上。

我们的技能不是固定不变的。你可以擅长很多事情。你可以做到任何事情。
