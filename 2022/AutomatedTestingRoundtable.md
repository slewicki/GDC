# Automated Testing Roundtable Day 1: Process

Andrew Fray, Lead Programmer at Roll7

## TL;DR:

Various testing processes exist. It's always best to start small and to start with the best tool to help solve the problem at hand.

## Summary

Different studios have different problems and with that various solutions in their testing process. From talks on how tests are run within various studios to how to handle tests with networking. Different teams will have wildly different resources, hurdles, and outlook. Start small and (as stated in other roundtables) have processes and tools meet the engineering problem "where it is" instead of taking an idealistic approach.

## Raw Notes:

Andrew has been running this roundtable for about seven years, this is the second virtual one. Please bear with and provide feedback afterwards. Today is about process, talking about the people that write the tests and the workflows and processes that enforce the tests. We don't stick strictly to this, other topics are allowed. Don't need to have special qualifications to chime in. Will collect topics initially and then walk through each. Link to website [Automated Testing Roundtables GDC 2022](https://autotestingroundtable.com/) that tracks these topics.

### Are there teams with a centralized service that has the target infrastructure for jobs and if so, how?

When deploying any infrastructure, is it managed on a corporate network fabric or something distributed? If it is distributed, how are you solving the network fabric piece for connectivity to the infrastructure?

- Deployed an automated testing rack infrastructure on-site at the Playstation Studios with a service that waits and accepts jobs. Once a job is accepted, it will package and execute the job on an available Playstation console.

- A pool of both PCs and consoles is made available to run tests on a multi-platform game. Tests are run on PC, but a console may be requested and deployed to. Once a test has finished, it can then be released back into the pool of available machines to be used again. The service that manages the pool of PCs and consoles is custom for in-house use (not TeamCity or Jenkins). In order to help run tests across various platforms, additional adapters were created to help provide commands in a manner that they may expect (such as handling copying of a file which varies per console).

- Custom service that performed full automated loop testing, that is testing that pulled from either the team or from a schedule. With the results the system provided allowed the team to perform call stack comparisons on crashes as well as stop the console that crashed and send an alert to the team that the console is currently in a stopped state for someone to hop onto the console and start investigating an issue. This was implemented using fuzzy matching. Meaning we would provide a call stack and if it didn't match within 80% to stop.

- We have a service running in Japan, the US, and the UK. The service is able to pick up any job from the different regions and is very much seamless over the corporate network. However, this is a huge internal IT pain with a huge cost associated with it. Something in the cloud could help make this a major win.

- #### Takeaways: Corporate fabric seems to be a similar pain-point for multiple studios.

### Testing for crossplay? How do you test for different platforms at the same time?

With mobile and platform crossplay how can interactions between different platforms be tested against simultaneously? How can simultaneous testing be done when a console is paired with another device like Android? Can the paired device somehow relay information to the console and vice-versa? Does the server become the puppeteer or is another process handling the device and console?

- Keep track of all data, especially logs. During testing you will want to know that the server is correctly handling all the different clients properly. This will help when trying to compare logs across their executions.

- #### Takeaways: No main takeaway, pain-point for other studios as well.

### How do tests communicate within the game?

Have any studios worked, or experimented, with running the game, but having another process send commands to the game to test? How and what were the results?

- Most tests we have are built into the executable itself or in a standalone DLL with all of the testing logic. This always for running the game with a command line argument will trigger the specified test execution. The results then get uploaded to a shared network drive or another service.

- We have some scripted tests that seem similar to the topic mentioned. These scripts will monitor and check various points that someone like QA would specifically ask to check.

- We have tests that will run on the PC and then connect to a console running the game. The test will then send tasks to the console such as firing a weapon or throwing a grenade. Afterwards we fetch the game state to perform validation that the commands sent were successfully executed. Of course, timeouts are implemented because we can't expect everything to be run instantly and we have to account for network latency. Regular expressions are used to determine whether the response was expected or not to then be handled accordingly.

- We also embed the tests in a DLL as we can just fire off the tests and forget about it. This way we don't have to worry about external processes sending commands and possibly other processes that need to monitor the full infrastructure. We can, instead, just deploy the tests to multiple consoles and gather all of the data at the end.

- #### Takeaways: Happy to hear everyone's responses and how other studios approach the problem, including embedding the tests within the executable.

### How to move from manual to automated testing? Where do you start?

We use Unity at our studio and have been primarily testing manually. We've been looking around on how to get started with automated testing, but there seems to be a lot of information out there which makes it harder to get an idea if this is how everyone performs automated testing themselves. How do we make the transition?

- Unity has their own testing framework which has a lot of good things about it and a lot of things left to improve. With this framework, you'll be able to tell Unity to load a scene, move this person, etc. You can even have the tests send a report to the UI or run it from the command line to have it output a JSON which other services, like Jenkins, to pick up.

- Unreal Engine 4 has a plugin for [Machine Learning](https://docs.unrealengine.com/4.27/en-US/API/Plugins/UE4ML/). It's an experimental plugin that currently doesn't have a lot of support, but it provides an example of how to run outside automation tests. There is a Read Me for the actual documentation. Essentially, after you turn on this plugin, it will build all of the sockets and ports in order to talk to your game. It's a python controller that has a loop used for machine learning, in this case reinforcement learning. It solves a lot of problems and provides an example on how to set up the client, server, and the code to automate your game from the outside. This provides a way to have a shareable interface into your game to have a way for inject actions into your code.

- When I was brought onto a Unity project, the game was already released and pretty stable so we had to look for other options and stumbled upon [Poco: A cross-engine test automation framework based on UI inspection](https://github.com/AirtestProject/Poco). This allowed us to perform image comparisons which was extremely easy and allowed us to interact with the hierarchy.
  
  Unreal's built-in testing framework, Gauntlet, is 3 things in 1. The Gauntlet framework us a C# project that controls the Unreal session. It asks about what you are, where is your build, what kind of build are you, if you are a client or server, are you running on a Windows machine, and manages all of the session's logic. Then you have the Gauntlet plugin which is the C code that talks to your game. It's the plugin that orchestrates your tests. It claims to be able to run on many devices such as Playstation, PC, mobile, and whatever else by handling the abstraction to those devices, but it has issues on Mac connecting to iOS. So if you want to automate on iOS, you will need a Mac or perform some tricky hacks. Otherwise this is a very powerful system as it's in the same place that the developers are writing their code. This allows the devs to start writing their tests right there and then it will be connected to all of the classes that they are familiar with. It exists in the editor, the engine, and integrates very well with the other tools that Unreal provides.
  
  #### Takeaways: Lots of points and methods to try out for both inside and outside automation

### Should only developers be the ones writing tests?

Does it benefit the team for non-developers to write the tests? Are developers writing tests the gold standard?

- Both the engineers and QA analysts should be writing tests, but to me, those are both types of developers regardless of being from the traditional dev side or from QA.

- Designers should be given the proper tools to write tests. They're the ones that are coming up with how things should look and feel like. If there's a system that should be doing one of the big goals, then the designers can write the tests in something like Unreal's blueprint and be used through blueprint as the designers sometimes already know how. This is something that can be taught to QA very easily. Everyone should always be testing as it makes everyone's life easier.

- Everyone can write the tests, but does that mean that everyone can take action on them? This introduces an inefficiency as the team may not be able to address the problem. Instead the team becomes a middle person where they will not be writing their own tests. It needs to be a balance that if you understand how to write a test, you should also be able to understand how to take action against a failing test and not just turn it off.
  
  ##### Other Resources
  
  [unium: Automation for Unity games](https://github.com/gwaredd/unium)
  
  #### Takeaways: Balance of responsibilities between writing and handling of tests.

### What is the granularity of a typical test?

Are there any good examples or references as to how granular a test should be or what metrics it should focus on? What would be a good first test to start with? Can you have a test that records player input and interaction?

- Coming from a game engine with no prior tests, we started to add tests that wouldn't block user flow or tests that wouldn't initially break the engine for the developers. We started with tests that would block integrations. That way if a test failed, it would not get added into the game engine as we didn't want to block users from any disruption from the game.

- The simplest of tests to start with is a boot test. Make sure that the game is able to open up and start. That the client is able to shut down without any crashes. Having this test running frequently can help catch any issues that could cause breaks and crashes. Other than that, some simple unit tests such as a new class that has some simple jump mechanic. You would create a unit test that would have the jump functionality triggered and then you would check if the position of the character matches what the expected effect would be.

- Integration tests are also great to test specific client side logic. This can be done by creating a simple scene that tests a certain aspect of the game. This helps test everything that can't always be easily tested in QA.

- We actually have a test where the game loads with a character. The character then proceeds to perform a bunch of actions such as running around, jumping over certain aspects of the level, fires, changing their weapons, etc. So you can get quite detailed in the interactions of the character within the test. Unreal has a system for this called [Replay System](https://docs.unrealengine.com/4.27/en-US/TestingAndOptimization/ReplaySystem/)

#### Takeaways: Great places to start and look into.

# Automated Testing Roundtable Day 2: Legacy

Andrew Fray, Lead Programmer at Roll7

## TL;DR:

If you want more stability then more testing is important. Simplicity is the key to brilliance.

## Summary

Getting testing into the project requires some political capital that can be done by helping optimize everyone's workflow. Either by simplifying existing processes or enhancing and adding value to current ones. Data can provide detailed analysis as to how introducing an automated process will help benefit the team as a whole.

## Raw Notes

Today we deal with changing the culture of a code base which doesn't have automated testing, or has resistance to it.

### (Discord) How to get the developers to start writing their own tests instead of other teams.

Usually testing is focused on QA and other testing teams, but is there a way to get the game developers excited to write their own tests? How can we introduce and teach testing to the game developers?

- Testing is 2 folds. 1) The state of the game is stable. Meaning that no one submitted any bad data that can crash the game or the build pipeline. 2) automated testing. There will always be some resistance from the game developers to create more tests. They would generally add tests if something would bite them quite hard. The biggest cultural difference is that either the game teams don't know the toolset or the language. Coding in the same language or same code set is usually not a big deal. Met with questions from the team of why should we do this or how does this benefit me.
- One of the things we did while building up the team,the tech, and the culture of automated testing, Initially we wanted to keep everything very focused. We created these walled gardens of things to be tested. Like enemies will be tested, the player will be tested, and everything else will be in a free for all. The team is now mentioning things like how the UI libraries should've been tested or the challenges system missed not being tested. It warms my heart to see the team mentioning these things.
- After 3 weeks of the builds just breaking, the team woke up and said that testing was needed. This all woke up the studios' eyes as to how important having testing is. Baptism by fire.
- I was the main point of resistance at our studio and it was argued long enough until I had caved, but now I can't live without it anymore. Once things were breaking and things were taking too long which led to more time taken during manual testing, we wanted to automate some of our issues. I couldn't afford to test hours and hours of gameplay manually every time I wanted to deploy.
- Our environment artists didn't want to test before checking in as it would take them 30 minutes. 7 out of 10 times, when they checked in, everything would break. They didn't want to lose 30 minutes, but instead wasted weeks of the rest of the team.
  
  #### Takeaways: Eventually people will be onboard with testing whether early on or by some issue causing wasted time.

### How to set up automated testing?

From a small engine studio, how does one set up automated testing when nothing is set up yet? What tools are used? Can FPS be optimized from these tests?

- What worked pretty well for us was [Poco](https://airtest.doc.io.netease.com/en/tutorial/3_Poco_introduction/). It worked well and was easy to set up. We used python scripts to run the tests which hopefully works on your project. From there I would focus on finding a repetitive task and automate that.

- Anything repetitive can be automated as well as anything to get metrics such as performance or memory that can then be saved to a database. Another quick and simple test to automate is a smoke test or making sure that the game loads. For a smoke test, pick a level that is used most of the time. Or if a milestone is coming up and something is getting altered frequently for a deadline, pick that as the basis for the smoke test. You pick the game mode, the player, and a couple of AI objects, load them all into this room and have them jump, shoot, crouch, etc. Just a small and simple set that you know could break the actual testing. If you have this automated and running on every build, this helps prove that your builds are stable. If anyone submits something it's much easier to tell if something breaks along the way.

- Another thing that can be done is to record player inputs. With this you can load the game and then load the inputs that need to be replayed. Only problem is if the designer changes something in the level then the player can get stuck and that's a problem. But this can be validated with screenshots and other forms of validation to help alert you when the level changed and fix up the inputs.

- If a bug is spotted in your game, you can create a test for that by using the steps to reproduce the bug. This will not only help validate the fix, but also prevent regressions in any future changes. Useful for when a change unexpectedly impacts other parts of the game.
  
  ##### Other Resources
  
  [What options do I have for Automation and Unit Testing In Unity? - Unity Forum](https://forum.unity.com/threads/what-options-do-i-have-for-automation-and-unit-testing-in-unity.682720/)
  
  #### Takeaways: Great simple places to start with room for adding on

### How to test rendering such as expected pixel and screen output?

What is the best way to test what can be expected to be rendered on the screen to have the correct color and expected output?

- Image classifications are a great way to perform these tests. Checking if an image is within 80% of a threshold is a good metric as you would be able to use this when something like lighting in the scene would change. This is in respect to pixel and color density. Should something break, you'll most likely notice a change in color and contrast. Chances are if the engineers break something horribly, the screen would mostly be black.
- AWS uses a device farm to test on both Android and iOS. We have functionality to take video and be able to take screenshots at certain points of the video. From the screenshot, you can then use whatever tool you have for validation.
  
  #### Takeaways: Setting up a comparison threshold is a good way to check if anything change in the rendering

### How to incorporate tests in an already existing and released product?

What could you say to convince higher ups such as managers to help convince that testing is worth investing in? Especially if the game is already out. The game is making money so what can we do to get resources in testing?

- Provide data to the higher ups as to how testing can help cut down on costs. We created a report that showed the costs saved when using automated testing when compared with manual testing from QA. In our studio QA is generally a bunch of contractors so we were able to generate data of how automated testing can be of value. The report can be a general breakdown of a recap of the failures and the cost differences between automating those failures and giving it to manual testing.

- Showing data helps especially with recurring issues. If something happens more than once then it'll need to be addressed. Why is it happening multiple times? Is it the people breaking stuff that need to be fixed? One of the ways I got this data was I recorded the start and end of a player's session. This gave me information such as when the player got disconnected, the server crashed, or the player themselves crashed. What these stats provided me were that PC players were less tolerable than say console players. I noticed that 20% of PC players that experienced a crash, would not play the game again for another 20-24 hours. But in the PC space, gamers have a lot of options. So if a player is not connecting or if they're experiencing issues in say Fortnite, then they'll just load up League of Legends or something else. I've seen people experience issues or there's maintenance and they'll step away from the game for days, sometimes weeks. If you wish to have this constant player engagement then stability is key. If you want more stability then more testing is important. Showing management these types of stats can help you make your point.

- Performance testing is one of the bigger ways to advocate for automated testing. In most cases, developers will release the game without automation and then come back with something like "Why do I need automation? I've done things this way my whole life." Usually the argument that I've seen in this case is that you never want to release a game where performance is not acceptable. So I'm putting it in the perspective of not just testing the mechanics of the game, but actually testing the whole game itself. This helps persuade them in a way that doesn't work when trying to get them onboard with unit testing or other forms of testing.
  
  - My last studio launched a game which was a complete disaster due to performance. We didn't have any performance testing, but I actually had to go and get a 3070 just to run our game with 30 FPS. This was a game that was targeting consoles so it was already too late to try and fix it since we were out the door. This was a live, free to play game, so no money. Basically, if we had some testing in place we would've been able to mitigate this. What we found from late manual testing was that the bushes had over 1,000,000 triangles each. It's things like this that could've been caught early on and saved your game.
  
  #### Takeaways: Data is key in helping provide useful analysis.

### How to speed up testing when adding tests onto an already existing codebase?

In a studio with an in-house engine and project, tests were an afterthought. The application would startup, attempting to load everything for a simple test before tearing down and repeating the process for the next test. How can this process be sped up and made more efficient without the mention of a heft refactor?

- Split up some of the longer running tests. Maybe only some of the tests need to be run frequently and the others can be run nightly or over the weekends. This may lead to a lower cadence, but there should still be some meaningful results.
- I am currently building a new testing automation system and there are some existing tests that run 2, maybe even 3 hours. So there are 2 ways we can look at this, either as a tools engineer or an automation engineer. You can try to parallelize the tests to try to get everything done faster or you can try and get the team together and get them to figure out a way to improve these tests. Ask them to do the work. If a test is taking hours, switch it to be run nightly or once in however number of days, but then approaching the team you can say that if we want more accurate results that we need to address these long running tests. Some other studios have a hard limit, like a test can only take as long as 5 minutes to execute. Another problem is that those that write these long tests usually just want to see results. They wrote a test, ran it overnight on their machine, and got results. So to them this is all fine. These tests should not be pushed into your systems. The testing systems should be placing some kind of restraint so that it helps free up resources. You should be the one teaching and providing guidelines on how to make effective tests, but never sign up for refactoring and trying to fix someone else's test. That's a nightmare scenario.
- Try to train the people on how to write tests. Perhaps look into different testing libraries, various books, or talks about best practices in testing. Try to aim for a simplistic approach. Provide the team a template on how to write an efficient test. All that would have to be done is copy and paste the template and just set up the data to be tested. No one has to learn a new function and no one has to learn a new language. An example would be that if you structure this JSON file with a certain set of rules, it should then just work. If it's something that's not supported, only then should you reach out to me and I can help add support for it. Simplicity is the key to brilliance. If the barrier to entry is low enough then other people will do this and they can do this without your help. These are the best systems.
- The question is why do the tests run so long? Sometimes it's the system itself. For example in Unreal, you have to load Unreal. So it's not the test itself that's really slow but the game. So we had to frame the question of how we can run the tests faster. Instead of using a map that is heavy and loads everything, choose a simpler map that takes less time to load. This will help optimize everyone's workflow and allow for some political capital that can be used to persuade them on other potential improvements.
- If the tests are running slow, try to split up the tests into groups and run them across different machines.
  
  #### Takeaways: Optimize the tests and provide guidance to the team with templates and system restraints. Simplicity is the key to brilliance.

### Is it more time consuming to write the tests or to find the correct automation tool?

- It depends on the engine. New World was built using Lumberyard and there was no testing framework built into it. What was done was debug like commands were built so that the automation team can orchestrate what needed to be automated. Another project was done in Unreal, which has a testing framework built into it. It really depends on what is being used and what's available.
- It depends on the available resources. Depending on the team size could change what can be done. Whether part of a giant corporation or small team can also change the amount of resources, both financially and physically. The amount of tools available are vast. You have tools with Machine Learning, tools that go deep into your game and analyze everything. The conversation changes if the game was written with testing in mind, but in the end it all comes down to how resources can be adjusted for tests without degrading other services.
- It also depends on the team and if they're all set in their ways or if they're willing to adapt. If they are set in their ways then you may not be able to fight it. In my last studio, I didn't write many tests unless something irritated me. I sat down with the team and they're showing me how to make their builds. They show me the process where they check if all the files are there and to check if the files and past files were there to make sure that nothing is being incorrectly merged. It would take 1 person doing this for about 6 hours. I took initiative and made a tool within my first week. In my mind I was not going to do this but instead automate it. When I presented the tool to the team, they all said that they would do it manually and to this day, even with all of the documentation I had left them, they still refuse to use it. I still see their social media posts about crashes and things breaking and I know this is because they're still doing this all manually.
  
  #### Takeaways: Work with the tool that's best suited for the problem at hand.

### How do you prioritize between testing and the backlog?

In my team, the backlog is always a constant struggle. We need to test <u>X</u> but <u>Y</u> has cropped up and should be tested instead. What are some approaches to handle this that others have encountered? Adjust priorities? Hire contractors? stopping development?

- As a PM I've experienced all of the above in terms of solutions to the backlog and scope creep. The phrase "It depends" is a cop out, but you need to prioritize the most important thing at the time. If too many new priorities crop up, "backlog bankruptcy" is a valid option to use and start over.

- When I was in the RM role, it was a priority and an ability to cut other things that were wanted in order to get the needs out. An example would be if testing was a need, but a new level was a want. I'd make sure to cut out the level and ensure proper testing was in place.
  
  #### Takeaways: Prioritization helps bring focus on what's important for the project.

# Automated Testing Roundtable Day 3: Implementation

Andrew Fray, Lead Programmer at Roll7

## TL;DR:

Code as water going through some pipes can be used as an argument for why testing for leaks and problems early on can be beneficial.

## Summary

Continuation of some more topics on trying to get tests adopted within the teams. How can we make some compelling arguments to prevent bigger issues and how can we help guide other members of the team to achieve the same goal of a stable product.

## Raw Notes

Today we talk about resources and best practices on writing good and effective tests which can lead us into helping our team adopt these practices as well.

### Good resources on testing?

Are there any good books that got you started in testing that you found to be excellent? Any good resources that you still find  today or a formal way to learn the practice?

- One of my favorite books is [The Art of Unit Testing by Roy Osherove](https://www.artofunittesting.com/). It's got a couple of additions like one for C# which would be useful for Unity developers, but it's not really focused on any single language. It really focuses on what makes a good unit test, how it is treated as a separate discipline, and how to write them.
- I don't think I've ever read a book about Unit Testing with regards to games. While there are a lot of books on how to write unit tests, they terrify developers. Everything about them feels very muddled. So what I've been training people in is the [International Standardized Testing and Quality Assurance](https://www.istqb.org/). They have a standard language that talks about testing and makes it easier to communicate this with others. There are also talks about testing at GDC which have been very good as well as the resources provided by Unreal.
- Looking at the documentation and tutorials of engines with integrated testing like Unreal and Unity can be a good place to start. Other than that a search of some testing frameworks for the language that is used in your game or engine would be a good place to start and brushing up on some documentation for those. For example C++ has Google Test, Catch2, and Cucumber as options.
  
  #### Other Resources
  
  [Complete Guide to Test Automation: Techniques, Practices, and Patterns](https://www.amazon.com/Complete-Guide-Test-Automation-Maintaining/dp/1484238311)
  
  [Growing Object-Oriented Software Guided by Tests](http://www.growing-object-oriented-software.com/)
  
  [The Phoenix Project](https://itrevolution.com/the-phoenix-project/)
  
  #### Takeaways: Books, GDC talks, and using what's built into the game or engine you're working with are some good starting areas.

### Measuring test effectiveness?

How are tests usually measured in regards to how effective they are? Is it by bugs found? Hours ran? Something else?

- The effectiveness of a test is like running water down some pipes. You want to make sure that the water is going through all the pipes so that you can be sure that there will be no issues later on. This is what you would be trying to accomplish within your test. To make sure that you're maximizing your code coverage which would cover all edge cases and general functionality.

- Scoring the test executions themselves can help measure the effectiveness of the test suite. I am currently experimenting with the formula of <u>(# of regressions / # of runs) - (false negatives / # of runs)</u>. The assumption here being that if you run the tests a bunch of times and it catches regressions a lot, then it is really good and thus gets a positive score. On the other hand, if you find it needing constant tweaks because of something you changed, then its probably requires further investigation.

- Have an inspection loop every so often for your current test suite. If the loop finds that the test is no longer doing what it was supposed to, then there should be an option to turn them off. We think a lot about starting and adding tests, but not much about deprecating tests. Sometimes you just really need to make sure and see if the test is still valid. There's no point in keeping a test around if it doesn't provide any value.

- Don't place too much focus on bug count. This puts too much emphasis on tests that generate bugs which will then lead to duplicate bugs being filed. With tracking hours, you'll just be adding on tests that keep running for hours with most of them not really providing much value.
  
  #### Takeaways: Code coverage helps maintain proper functional implementations and scoring can help point to potential weak spots.

### Are tests run with Sanitizers enabled?

For those who have game engines written in C/C++, do you run tests using a build of the engine with sanitizers enabled? For example AddressSanitizer, UndefinedBehaviorSanitizer, etc?

- Our custom engine runs our unit tests 3 times. First run through is without any sanitizers enabled to catch any immediate issues. The second time is with the Address Sanitzer enabled, with an attempt to recover so that we can get a stack trace printed out within our build logs, and the third time is with Thread Sanitizers enabled to help catch any potential data races and other architectural anomalies.

### How to convince others on the team to add tests?

In my studio, it's been an uphill battle to convince the team to add some validations as a step between commit and visibility. What can I do to arm myself for the arguments that are against this.

- A leading argument could be if something is terribly broken because there were no automated tests of any sort to test this particular scenario. Showing this can help be an easy case to prove why it's needed.
- Some time ago at GDC, there was a talk about to automate a game that was basically running on it's own [GDC Vault - From Live to Sunset: Automating Live-Ops and Game Services](https://www.gdcvault.com/play/1027079/From-Live-to-Sunset-Automating). This can help persuade to team in how there can be value in automating this and allow us to move onto other projects while the game is still properly functioning and delivering content.
- One way to look at it is every day that your builders are down is testing time lost with the manual testers. It's also time lost with the developers are they should be getting the latest and continuing their work. Now everyone has to do double the work to get the build back into a working state. My last studio did the math and recognized about $5,000 per day in testing time. Another point to bring up could be that if a live service goes down, it could be losing hundreds of thousands of dollars because of an issue. We also have a system in place that runs these validation checks on every commit. These checks are run on a farm which takes no resources away from your machine so you are still able to keep working. Once everything passes it will automatically perform the check in for you.
  
  #### Takeaways: Testing can improve a project's stability.

### How to get more developers to write tests?

Very few tests at my studio are actually written by the developers. What tips are there for getting more people that don't have test in their job title to still think and write about tests?

- I came from QA and a lot of people I know came from QA. As I got higher and higher up in the chain, I noticed that the upper management will disregard anyone with test or QA in their job title. They don't treat them as being part of the team nor do they take them seriously. Lately it seems that the culture has been shifting and we see it in GDC where it's mentioned that testers are the mainline defense. That we need to start taking them seriously and treating them as a part of the team.
- What helped me get developers into testing is to try and have them think about tests as part of self documenting code. When going through code reviews, it's easier to look at a test and see what it is this new feature is trying to do. What are the passing cases, the failing cases, and any edge cases that are handled. This helps give us an overview of what we should be expecting in terms of the actual implementation. It also helps bring on new members as we can point them to the tests as to what it is our engine can do.
- I was sneaky in how I went about implementing my tests. When I released my first features, I released them with tests. One of the senior developers got excited because they saw that someone actually put tests into the project.  It's probably a bit harder to convince the small and intermediate teams, but once you have some people it'll be easier to do so. Especially if you have some supporting data.
- What worked for me is to find a person that can be a spy inside the team. It's easy to find this person as they're passionate about everything. So we started with simple talks about static analysis and running tests that showed 5% of code coverage on our dashboard and this starts to get everyone talking about how we can get these numbers up. From there everything just starts to evolve and we're now at about 90% code coverage.
- What I found useful was a [GDC talk by Robert Masella in 2019](https://gdcvault.com/play/1026042/Automated-Testing-of-Gameplay-Features) about test driven development. It shows a graph with the development time on the X-Axis and the Bug count on the Y-Axis and you can see this nice steadily growing curve trending upwards when speaking about Banjo Kazooie Nuts and Bolts. The line dips a bit after Beta but before launch, but when it switches to Sea of Thieves, there is a consistent flat line. You can hear a buzz in the room and after showing this to my team, everyone was getting pumped for what it could do for us. It's not just you twiddling your thumbs while developing a feature. It's helping mitigate all possible risks.
- #### Other Resources
  
  [Automated Testing at Scale in Sea of Thieves](https://www.youtube.com/watch?v=KmaGxprTUfI)
  
  [Automated Testing of Gameplay Features](https://gdcvault.com/play/1026042/Automated-Testing-of-Gameplay-Features)
  
  #### Takeaways: Finding an ally can be a great way to start a discussion. Self documenting test code can be a great way to bring members on board, especially during code reviews, and code coverage data can help with the push.

### How is the organization structured?

Where I work, we have the game team and we have the separate QA side? Are the structures different in other studios? Does QA write the tests? Do they not write the tests but are still in charge of quality assurance? Are they separate or part of the same team?

- In my team both the developers and the QA are part of the same team. Everybody works together and everyday we talk with each other. I prefer this approach especially if it's a smaller team. I think this approach works well for us even in a virtual environment.

- Our team recently restructured so we are no longer embedded. We're still struggling with the Pros and Cons of this arrangement. One of the benefits of not being embedded is the feeling of a bit more free will. To work on things that could benefit the entirety of a team instead of a specific feature. It can also allow for better career growth as there's a place to return to when a game gets finished. It allows for promotions and growth that is longer than the project.
  
  #### Takeaways: No main pros or cons, but a balancing act.
