---
title: Final Evaluation - A time to look back.
date: "2022-09-12"
description: It has been 5 months since I determined to nail GSoC, 4 months since I got selected into RADIS, and 3 months since I started coding intensively. Now, it's time for the grand finale.
---

![logo_OpenAstronomy](./logoOA.png)

### 1. What I have done in OpenAstronomy and in RADIS

Throughout 3 months with RADIS, I have successfully developed a new fitting module for spectrum fitting purposes. RADIS has its own fitting feature, as shown in [1-temperature fit example](https://radis.readthedocs.io/en/latest/auto_examples/plot_1T_fit.html#sphx-glr-auto-examples-plot-1t-fit-py), where you have to manually create the spectrum model, input the experimental spectrum and other ground-truths into numerous RADIS native functions, as well as adjust the fitting pipeline yourself.

Now with the new fitting module released, all you have to do is to prepare a `.spec` file containing your experimental spectrum, fill some JSON forms describing the ground-truth conditions just like how you fill your medical checkup paper, call the function [`fit_spectrum()`](https://radis.readthedocs.io/en/latest/source/radis.tools.new_fitting.html#radis.tools.new_fitting.fit_spectrum) and let it do all the work! If you are not satisfied with the result, you can simply adjust the parameters in your JSON, such as `slit` and `path_length`, then recall the function again, until the results are satisfied.

This is way easier and more convenient than dwelling into RADIS documentation to find out and learn how to use the current example, especially for new RADIS users. Various [benchmarking efforts](https://github.com/radis/radis/pull/522#issue-1365475821) have shown that this new fitting module has performance advantages over the old version. This new fitting module aims to provide an end-to-end fitting experience, with minimum amount of RADIS knowledge needed.

You can see an overview of my project here: https://github.com/radis/radis/projects/6

### 2. GSoC, RADIS and a learning curve that has been fulfilled

Throughout my GSoC journey with RADIS and working as a contributor, I really enjoyed all the experience of developing and contributing a meaningful improvement to a grand community-based project. Furthermore, I used to be a Computer Science majored student back in Vietnam, but after coming to Japan, I have been learning engineering for more than 2 years, enough for me to miss the old time coding projects and grinding hackathons, the days when I was truly a “CS student”. GSoC truly granted me a precious chance to rekindle the interest I have long lost, with wonderous opportunities to learn from prestigious mentors, and a huge boost for my background to get back to the run.

In addition, one of the most satisfying moments in this GSoC, is when I finally nailed a bug or issue after days (or even weeks, trust me) of debugging, using the last brain cell to figure out what is the reason. The longer the suffering, the greater the hype that comes afterward. I guess we as developers all share this kind of experience often, but for a guy who starts coding again after a long time like, the ecstasy is at least three-fold.

Furthermore, I don’t know what other open-source projects are, but RADIS is an extremely well-developed one. They have code coverage, pre-commit check, automatic documentation, and an extensive library of well-structured classes and methods for multiple purposes. This is a level of professional development I have never seen before, and I am extremely eager to learn all of this, not only within the GSoC, but also for a much longer time. This project also helped me to gain significant knowledge and experience 

So, to sum up, I really enjoyed this GSoC, especially with RADIS mentors and community. Thank you so much, GSoC and RADIS, for all of these wonderous experiences.

### 3. Of course, there were hard times, but hey, "Hard times come again no more"

I believe the most challenging part of my GSoC 2022 experience, is during the development of my project itself. My project is “Spectrum Fitting Improvement”, in which I will implement a brand-new fitting method that uses a different module than the original method’s, and there are several challenges that I only discovered after joining the project. 

Firstly, the fitting process itself is totally a black box, where I implemented a spectrum, along with its ground-truth parameters, and hopefully the result comes as I expect. In the early days, there were weeks when I could not understand why the result went bad. The reasons could be faulty ground-truth data (original ground-truth parameters are incorrect), or the spectrum itself (mistakes during spectral variable extraction), a code bug, or even from the RADIS limitation itself (currently RADIS only uses air broadening coefficients, which is not suitable for experiments in other gases). All of these costed me huge time and efforts just trying to figure out the culprit, and those were the most anxious times.

Secondly, there are several problems and bugs, or required implementations that can only be discovered during the last weeks of this GSoC, which makes these time tough and sour for me.

Finally, my laptop was abruptly broken beyond repair during the middle of second phase, in which I had to wait for one week before the new laptop arrived and I could continue my work. Truly the darkest, most desperate days back then.

Gradually the learning curve is flattened, but still, there were tough times. Thanks to GSoC, I could experience what would happen in a real project, where you have to anticipate and be ready to deal with all possible accidents and troubles, while keeping on a tight schedule. These will be precious experience for me and my career ahead.

### 4. A little tribute to my mentors

Firstly, I would like to say, thank you so much, my mentors - Mr. Erwan, Mr. Minou, Anand, Gagan, as well as other unofficial mentors such as Mr. Corentin - for all the time and efforts you have put through to guide us – some random annoying students always trying to bother you with questions throughout 4 months. 

You helped us a lot in understanding the RADIS codebase and overall structure, as well as various skills in developing a grand-scaled project like RADIS. Throughout this GSoC, I had opportunity to familiarize with code coverage, pre-commit check and linting, automatic documentation such as readthedocs, a bunch of GitHub tips, and most important, a sheer confidence of open-source project contributing, by jumping into the source code itself, understanding it slow and steady, then finally pushing commits. Before this April, all of these were very scary for me. But now, as I look back, they are just breezes to me. Now I can truly understand and feel the scope of GSoC – to encourage students to contribute to open-source projects. Thanks to you, this is a huge success to me.

All of these could not be done without you entrusting us from the very beginning of selection process. From the very moment of you accepting us, we are here today, wrapping up what we have learned, finishing our projects, and carving our names into the list of RADIS contributors. These will be precious experience for me and my career ahead.

### 5. And finally, to someone reading this

Ayyo, to whoever reading this,

I believe that you must be some next year's GSoC applicants sneaking around and patiently preparing for the upcoming turn. If you are reading this, then firstly I would like to say thank you for reading all the way here.

I have so many things to share you about all the experiences I had during this GSoC, about every moment in all aspects during these 3 months. But I’m afraid I might accidentally spoil your fun in near future, so I will only give some necessary advice, hope they might help you enjoy better in the next GSoC.

Firstly, the actual time required to complete the project always LONGER than the initially planned time, so try your best to finish everything as soon as possible.

Secondly, there might be times when you confront an extremely hard issue which takes you A LOT of time and you still cannot deal with it. When that time comes, explain to your mentors, and find a way, instead of gazing on the screen trying to solve it singlehandedly while wasting 1-2 weeks for that, like I did.

This is also relevant to the above advice but, if you find yourself scared to tell your mentors about a challenge you are facing, please do not be afraid and just tell them. I used to be extremely afraid of asking my mentors because sometimes they were deadly serious (in a professional way), and thus I forced myself to solve an impossible task for 2 weeks before finally reaching out to them. Please do not be afraid and share with them anything, if you want to find a solution, if you want to change the current objectives, or whatever. Just ask!

And finally, try to enjoy GSoC, I meant, every moment of it. It worths. Really.

Good luck to become a GSoC member and successfully carve your name among contributors!

  
September 12, 2022
Tran Huu Nhat Huy

  
![Me, among the peaks of Shizuoka, Japan.](me.jpeg)