---
title: January Postmortem
image: https://live.staticflickr.com/65535/50878449367_e2d0493142_o.png
categories: [learn]
tags: []
hidden: false
author: emmyoop
---

Reviewing how you reached your goals, if you reached your goals, and the **why** behind it all is just as important, if not more important, than actually setting those goals.  

It gives you the opportunity to reflect on how your goals panned out, how much more or less time it took you to complete those goals, and what you should do moving forward.  Over time, it also improves your time estimation to complete tasks as you get to see how long a task actually took instead of just guessing.

### January Goals 
1. Find and implement my own website theme
2. Finish the SQL for Data Analysts Course
3. Find and get set up for another [Kaggle](https://kaggle.com) competition


Starting from the top!

### 1. Find and implement my own website theme

I don't know if you noticed but my website is looking pretty sweet!  I love the new theme I picked.  I already wrote about the process in my [Progress](/progress) update.  Since then, I've fixed a few bugs in the website and made a few customizations.  By default my theme had the font color of links set to the same font color as the text.  It was very unclear what was a link or not a link unless you hovered over them.  So I dug through the CSS (with the help of Chrome Developer Tools) and updated it to flip the colors so links are much more obvious now.

However, that resulted in every single link being <span style="color:#de7a93">**pink**</span>. Even post titles.  It was not appealing so I had to dig a bit more into CSS inheritance rules to figure out how to customize what links were what color when.  It took me longer than I would like to admit but I am happy with the result and it was fun digging around for a bit.

I have started a bug and feature list in a [notion](https://notion.so) workspace.  I have two major bugs right now.  

1.  The "hidden" tag on posts does not work
2.  The image size of slide shows is not ideal and some photos are being cropped in unexpected ways

Nothing too major but definitely two things I need to look into.

The more I work on the site, the longer my list of features becomes!  One of the most exciting features I want to implement is being able to embed Jupyter Notebooks into my posts.

When I showed [@rocktavious](https://twitter.com/Rocktavious) my new site, he was a bit jealous.  He suggested I overhaul [Red Owl Games](https://redowlgames.com) but I think I have enough on my plate for now!

### 2. Finish the SQL for Data Analysts Course

{% include image.html img="https://live.staticflickr.com/65535/50877638938_9b79026d65_b.jpg" lightbox="true" alt="DataQuest SQL Course Certificate" caption="I did it!" align='right' %}

I've been dabbling with the SQL course for some time now.  Going back and forth.  I even took a break to review more SQL with DataQuest's new beta path SQL Skills.  Most of the course covered the basics of SQL with some more complex ideas such as joins, case statements, and using the with clause.  Toward the end of the course, they covered how to interact directly with your database in the terminal as well as with Python.  I've used Python to talk to databases before and a lot of that came back pretty quickly.

One thing this course did remind me of is how much fun it is to think through database design and figure out the best way not to duplicate data while keeping everything in an easy to read and understandable state can be a real (but rewarding) challenge.  At UT I was able to design quite a few databases and it was always an enjoyable task.

### 3. Find and get set up for another [Kaggle](https://kaggle.com) competition

Technically I did this.  I got set up.  Then I made zero progress.  I chose the [Rock, Paper, Scissors](https://www.kaggle.com/c/rock-paper-scissors) challenge and just did not have the skill set for it.

Recognizing what you can't do is an important skill to have when you're learning.  One of the most useful skills I learned at UT was that if you're working on a specific problem for 30 minutes and getting nowhere, stop and ask for help.  It's okay to not know.  It's okay to ask for help.  It's not okay to bang your head against the desk for hours at a time.  Although it is a skill to recognize the difference between "This is a hard task that I'm making very slow progress on" and "This is a hard task that I'm making zero progress on".

I made the call and backtracked to find a simpler contest to compete in.  The Intro to Machine Learning course combined with the house price estimate competition were a perfect fit.

{% include image.html img="https://live.staticflickr.com/65535/50838287883_1f1b6953d7_b.jpg" lightbox="true" alt="Kaggle Intro to Machine Learning Course Certificate" %}

### Bonus!

In addition to the above, I was able to get a few extras in this month.  Having dedicated days to work has really allowed me to use time well and really focus on specific topics.  Not feeling the need to do *all the things* in the 1 hour of quiet time I was getting each afternoon has really allowed me to focus on doing a single thing well.

- Started reading and working through [Think Stats](https://greenteapress.com/wp/think-stats-2e/), a free online statistics text book.  It takes me back to college.
- Went to [Bastrop State Park](/bastrop) for a fun filled family day
- Got my [notion](https://notion.so) workspace up and running so I can track bugs, features, post ideas and learning goals in a more organized way.
- Submitted my [IBM apprenticeship](https://www.ibm.com/us-en/employment/newcollar/apprenticeships/) application for both the Data Scientist and Data Analyst apprenticeships.  
- Joined a few discord servers for Machine Learning and Python.
- Switched to the Data Scientist Path on DataQuest.  It's more math heavy which is something I was missing in the Data Analyst Path.  It seems like everything I've done in the Data Analyst Path (65% complete) carries over to the Data Engineer Path (46% complete).  There are even a few calculus based courses after I finish the basic statistics courses that I'm really looking forward to.  


### Things I learned/realized this month

1. Man, I miss my kids when I don't spend all day every day with them.  They're pretty awesome little humans!
2. CSS inheritance can be complicated, especially when you are inheriting through multiple layers.
3. Math is fun.  Starting some of the work with statistics has reminded me of why I majored in mathematics in college.  Love.
4. One surprising realization is that working for prolonged periods of time means I need to make more of a conscious effort to move!  My body is used to hanging with 2 and 4 year old kids all day - not sitting at a desk.  Lunch time walks help.  Korra  has also been loving the walks alone with me.

Part of this postmortem of how January went is to establish some goals for February, so without further ado, my goal list for next month.

### Goals for February
{% include image.html img="https://live.staticflickr.com/65535/50878449337_382c3cfec8_b.jpg" lightbox="true" alt="Incomplete Course" caption="I need those check marks!" align='right' %}

1. Start My Spring Garden
    - compost
    - seed
    - trim rosemary
    - general leaf, debris, dead plant clean up
2. Family trip to Enchanted Rock.  I haven't been since I was in college and [@rocktavious](https://twitter.com/Rocktavious) has never been so it should be fun!  
3. Complete the APIs and Web Scraping In Python course in DataQuest
4. Complete the Data Analysis in Business course in DataQuest
4. Build my own (albeit simple) project.  I was thinking something along the lines of how (or if) the pandemic has affected Texas State Park attendance.  I'll have to see what kind of data there is available.
5. Finish out some random lessons I skipped over in DataQuest to strive for that 100% - review never hurt anyone!


I hope you enjoyed all the lists that this postmortem was composed of.  You gotta love a good list.  And that concludes my work for January!  it's been a fun month of change, learning, and brain exercise.