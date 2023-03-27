# Automated Testing Roundtable Day 1: Process

Andrew Fray, Lead Programmer at Roll7

Henry Golding, SDET Technical Director at Epic Games

## Theme

Beyond the software that we use to test our games are the people that 
write the tests and the workflows they use to reinforce them. How does 
the process work in your studio? Are the engineers responsible for 
automated tests, or QA? Are QA silo'd or integrated? What could be 
better?

## TL;DR:

It's always best to start small and early. Get teams on-board sooner to alleviate frustrations from QA and improve quality

## Summary

Different studios are trying to understand how to start with automated testing. From working in large code bases with multiple external dependencies to breaking the manual QA culture; starting with automated testing should be something small and simple. Creating a basic smoke test to see if the game runs can then be expanded upon and scaled to other features and functionalities. Getting different teams on-board can help alleviate the frustrations of testing and collaboration to more of a focus of a partnership.

## Raw Notes:

Andrew has been running this roundtable for about eight years, unfortunately, he was not able to attend this one due to COVID. Henry Golding, from Epic, has accepted the helm in his place. Please bear with and provide feedback afterwards. Today is about process, talking about the people that write the tests and the workflows and processes that enforce the tests. We don't stick strictly to this, other topics are allowed. Don't need to have special qualifications to chime in. Will collect topics initially and then walk through each. Link to website [Automated Testing Roundtables GDC 2023](https://autotestingroundtable.com/) that tracks these topics.

## Mentioned Topics

- How to get started with automated testing

- Teaching about automated testing

- Value of automated testing as a solo developer or a small team

- Testing of internal tools

- Who builds/implements automated tests

- How to improve and optimize existing tests

- How to approach breaking the culture of manual testing

- How to design and implement a good testing strategy

- State of AI tools for testing

- Demos to pitch for the use of automated testing

- Support of automated testing on legacy hardware

## Discussed Topics

### How to get started with automated testing

- When do you know is the right time to get started?
  
  - Start by investigating and discovering ways of optimizing the development process
    
    - When noticing that functionality is breaking, write tests around that functionality
    
    - Bugs that are a rare occurrence could be stressed by a test to try and attempt on recreating the issue.
    
    - Tests around bugs can be further used as validation that the bug has been fixed and can be used later to validate future changes
  
  - Focus on functionality used often within a large code base to prevent breakage in other parts of the code or dependencies
  
  - Try not to over complicate the testing process, start with something simple such as a Smoke Test
    
    - Does the game with a level, player, and other objects launch without crashing? Does the main menu load correctly?
  
  - Design testing around the pipeline and process. Try not to think of testing as an afterthought.
    
    - Testing is great to reinforce code quality
    
    - Tests can validate all functional scenarios immediately if taken into account during designing of the functionality.
  
  - Start with focusing on the most time consuming or monotonous aspects of the current manual testing.
    
    - Prioritize what needs to be tested or a lot of time can be wasted with very little return.
      
      - Localization
      
      - Large data sets
  
  - Get all of the engineers on board early as to not waste time later on in the development process
    
    - Present a pitch
    
    - Set aside time for training
    
    - Create a roadmap for the implementation of automated testing
  
  - Create a small hard-coded script to mimic simple player interaction within the game

### How to approach breaking the culture of manual testing

- Time tracking is a good metric to gather data
  - Wasted time spent on bug tickets
  - Time spent on testing repetitive and mundane tasks
- Human error is a factor that needs to be taken into account with manual testing
  - Pain felt by QA from testing frustrating, tedious and monotonous tasks multiple times or take an excessive amount of time to go through, can lead to lowered morale, which can cause errors
  - Novice or newer testers may not be fully aware of the scope of the systems are also a cause of errors that can be introduced
- Repetitive and mundane tasks should be covered by automation
- Bridge the gap by providing training and increasing familiarity with the testing tools to the manual testers
  - "Automated testing is a partnership"
  - Choosing the right programming language to avoid any barriers or which may cause a divide
- Functionality may be shared across teams and causing them to waste time by retesting the same functionality
- Established knowledge from manual testing has the possibility of being lost during turnover
- Manual testing is already using some sort of system which can be taken one step ahead and automated

### How to improve and optimize existing tests

- Run tests in parallel
  - Make sure that the tests aren't using any shared resources and can be isolated
- Long running tests to be scheduled to run during off-hour times
  - Slow points in the day such as early in the morning or late at night
  - Times when no one is around or working like weekends or holidays
- Caching of test data
- Optimize the process, focus on the issue and not the full test
  - Instead of having multiple tests loading the same level, have a way to test multiple functionalities reusing as much as possible
- Reorder tests to get the most recent or frequent test regressions to run first
  - Getting to a failed state faster
- Organize tests to run based on dependency order
  - If a test fails on a certain dependency, such as data, there's no point in proceeding further with tests that also rely on the same failing dependency
- Turning off parts of the system
  - Turning off rendering if testing physics behavior

### Support of automated testing on legacy hardware

- Split large test suite into several test slices to run parallel
  
  - Start with a hard-coded number of parallel slices, but look into spending time to making it more dynamic and optimal based on hardware configuration

### How to design and implement a good testing strategy

- Make sure that the testing strategy provides clear and concise error reporting
  
  - Taking screenshots of the failing state

- Proper tear down and cleanup of scenarios
  
  - Having artifacts linger over a previous scenario can lead to the following tests become flaky or fail

- Readability of the test code so that anyone can dive in and understand what the test is trying to accomplish

- Code coverage metric to validate that all functionality is properly tested 

# Automated Testing Roundtable Day 2: Legacy

Andrew Fray, Lead Programmer at Roll7

Henry Golding, SDET Technical Director at Epic Games

## Theme

As much as we would like otherwise, automated testing is still a rarity in our industry. Do you have a culture or a code base that doesn't currently embrace automated testing? Are you having trouble getting people on-board, or maybe it was very easy and you want to share why? Grand failures are just as interesting as successes

## TL;DR:

Features and best practices of a testing framework can help streamline testing and keep costs down.

## Summary

Investing in a testing framework with easy to use features can help an organization get started with testing. Testing comes with problems that need to be addressed, such as flaky tests, ownership, and costs brought about from initial setup and continued maintenance.  

## Raw Notes

Today we deal with testing frameworks and how to best create a process from initial setup with ownership all the way down to maintenance and upkeep.

## Mentioned Topics

- Features to look for in an automation framework

- Features that are in the Unreal testing framework

- Who will be writing the tests

- What is the biggest cost in automated testing (initial setup vs maintenance)

- How to deal with flaky tests

- Action recorders and playback in testing

- What features should be automated vs manual

- Test driven development

- What tools/middleware are used

- What platforms are tests being run on and reasons why

- Who owns the maintenance costs (who is responsible)

## Discussed Topics

### Features that are in the Unreal testing framework

- Engineers can write tests from the code within the engine itself

- Unreal has blueprints for those in a less technical role which is Unreal's visual editor

- Unreal uses [Gauntlet Automation Framework | Unreal Engine 4.27 Documentation](https://docs.unrealengine.com/4.27/en-US/TestingAndOptimization/Automation/Gauntlet/) to orchestrate the running and validation of tests
  
  - Can execute blueprints or something custom
  
  - Has a plugin called `TestController` which can look into tests from the framework itself

### Features to look for in an automation framework

- [Sikuli](http://sikulix.com/) can be used to take photo shots and using Python or another scripting language to automate testing
  - Can also be used as a tool to bridge the gap between non-technical and technical
- Clear and concise error reporting to analyze error logs
- Notifying the user of a failing commit instead of notifying the whole team
- Property based testing
  - Instead of testing `1 divided by 4`, the test can instead be phrased `Integer divided by Integer` and the test will generate a range of integer tests included 0
  - The [Hypothesis](https://hypothesis.readthedocs.io/en/latest/) library in Python supports this type of testing
  - Useful for catching corner cases
- Traceability
  - When working in an Agile environment and maintaining test cases, it may not be certain when a particular change would require updating other tests
  - Can help with test maintainability

### How to deal with flaky tests

- Figure out what the source of the flakiness could be
  
  - Run the test on a different worker to try and isolate a possible configuration issue
  
  - Run multiple times on the same worker to get data in order to try and identify a pattern of success and failing runs

- How to rebuild trust between stakeholders and developers caused from flaky tests
  
  - Tools can help capture metrics about the failures
  
  - Minor iteration or changes that weren't communicated on brought about flakiness in other tests
    
    - Tests that become flaky after minor changes should be addressed sooner rather than later as there is an underlying issue in the lower level architecture
    
    - Delete the test if someone is not able to reflect on what the proposed test is trying to accomplish

- Promoting technical people to handle problems arising throughout the day to help identify frequent issues
  
  - Can help describe error messages and provide statistics on what can possibly be the most erroneous cases

- Create a small bot to perform smoke tests across similar code bases

- Talk with the developers and designers and using the terminology of loose vs tight
  
  - Tight test is a test that is expected to return a concrete value
  
  - Loose test is a test that can return a range of values

- Flaky tests have the onus on the developer of the commit which also leads to frustration if the test becomes flaky on a piece of code they did not touch or work on
  
  - Place flaky tests in a specific category. Quarantine the test
    
    - Placing tests in a more verbose category can help identify a common underlying problem

### Who will be writing the tests

- Make sure that the organization embraces testing
  
  - Make this a core value
  
  - Have some sort of metrics to enforce
    
    - Code coverage cannot drop under a certain threshold
    
    - [Martin Fowler on TestCoverage](https://martinfowler.com/bliki/TestCoverage.html)

- Definition of done was born out of failure due to bugs getting let through
  
  - Write a unit test that covers the steps needed to reproduce the bug
  
  - Write a unit test that covers the functionality introduced by a new implementation

- Developers own the tests by enforcing code that cannot be merged in until it is reviewed by peers and has passing tests 

- AI programmer should own the tests
  
  - Games usually have AI bots which can be used to test gameplay mechanics
  
  - The bot essentially plays the full game by itself and can be used to detect issues
  
  - Simple test can be a bot moving between 2 points in a level and then validating that the movement was performed successfully

- Encourage developers to write unit tests, reasons they may not want to write tests is that they will have to write tests for dependencies and setup prior to writing their feature tests. Clear the path for writing the tests to have an easier method to write tests

### Who owns the maintenance costs

- Using technical practices to help scale the development of the testing tools

- What are the root causes as to why maintenance costs become high. Keep automation on as low of a level as possible

- Invest in bots to go through and test UI elements and functionality

### Test driven development

- To be avoided like a fire
  
  - Creates heavy drag due to maintenance costs
  
  - Requires separation of logic to have functionality unit testable

- Using TDD on APIs is desirable if the specification is decided upon

- Automated test created when a bug ticket comes in. Helps adding on to regression tests to build confidence in the bugfix

- Separation of the view layer from the game allows for easier testing of components

- Organization concept to help a developer reinforce what the end state is supposed to look like. This can cause development and the test to be done in parallel

# Automated Testing Roundtable Day 3: Implementation

Andrew Fray, Lead Programmer at Roll7

Henry Golding, SDET Technical Director at Epic Games

## Theme

Software automation has many faces, from locally-run functional and unit tests, to server-driven continuous integration, and even external device farms. How do you do automated testing in your team? What works well for you? More interestingly, what didn't work and why?

## TL;DR:

Exposing the different testing problems that crop up. testing on devices, mocking services/data, and scaling the build infrastructure.

## Summary

Testing for games still has some sore points that needs to be addressed differently from traditional software. Stress and performance testing large scale live service systems are shown to be resource intensive and cannot be run frequently. Testing on mobile has shown that each device can have their own quirks, but maintaining a physical device is expensive and VR headsets are still new territory to be improved upon.

## Raw Notes

Today we talk about resources impacted by automated testing. Whether it's external dependencies or lack of support for a specified platform, there's quite a bit of areas that can use improvement.

## Mentioned Topics

- Frequency of load testing live service games

- Where to start with a large legacy code base with no tests

- How to push non-automation engineers to write tests

- Service Virtualization to remove dependencies

- VR Testing device support

- Build  server capacity

- Mobile device testing

- Choosing which tests to be automated

- Scalable build farms

## Discussed Topics

### Frequency of load testing live service games

- Load testing is performed on every major release as end-to-end tests
  - Frightfully expensive to spin up and run, currently working on decomposing and converting the tests to become more modular to be run in smaller environments
  - End to end tests, while expensive, do capture critical bugs
- Run regularly as a development measurement
  - Run nightly on each new build
  - More strenuous load tests run over the weekend to feed data to a telemetry pipeline to help visualize the data
  - Useful to compare data between new development and what is currently in production
- Distinguish what part of the load test is to be run more frequently to catch more critical issues as often as possible
  - Save more stressful or resource intensive load tests for rarer frequencies
- Run more frequent load tests on a smaller scale to see if anything was broken

### Service Virtualization to remove dependencies

- Mocking of external dependencies to test against communication with those services
  
  - Game has an external service that handles payments that is not handled as part of the team. Mocking the response from the service can help test the games functionality without being dependent on the actual communication

- When running tests, 90% of the time it's testing the functionality and not the communication between the services
  
  - For the other 10% of the time, a smoke test to validate the connection can be a simple and clean way

- When mocking an external dependency, it needs to be made clear if the data is coming from the actual service or from a mock. Information should be properly documented to help diagnose any issues

### Where to start with a large legacy code base with no tests

- Start with simple functional tests

- Maximize coverage by cherry picking parts of the code base that is used the most

- Mob/pair programming to investigate an effective test
  
  - Being in a group promotes collaboration and ways to improve on a test

- Talk with the manual QA team as they already have some steps
  
  - Useful as the initial building blocks on what to automate

- Dependency breaking techniques
  
  - Find a component with dependencies to a lot of other components and extract an interface that matches up with another component. Use a mocking framework to make a fake version of that component to have the script referencing the interface rather than the component itself
    
    - [Working Effectively with Legacy Code](https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052)
    
    - [Code as a Crime Scene](https://www.adamtornhill.com/articles/crimescene/codeascrimescene.htm)
    
    - [CodeScene](https://codescene.com/)

- Look at runtime data to get information to start with, working on a system that works with player input

### How to push non-automation engineers to write tests

- Cucumber to give manual QA to write out steps in Domain Specific Language to then give to the developers to implement
  
  - Testers took some time to pick it, but it was easy to convert steps

- Frameworks that are action driven requires proper engineer support. Many attempts, but hasn't really proven to work. Requires maintenance and not just one and done

- [Eggplant Functional](https://docs.eggplantsoftware.com/ePF/eggplant-functional-documentation-home.htm) has a solution that operates over VNC connection for the tester to create a test macro. Proprietary language which seemed fairly easy to use. Similar solution with an MIT license, is [Sikuli](http://sikulix.com/)

- Implementing a replay system that can record actions and replayed in tests to validate functionality
  
  - Referenced in `Independent Games Summit: Fixing Bugs by Cloning them in 'The Last Clockwinder'`
  
  - [Smart Logs | Utilities Tools | Unity Asset Store](https://assetstore.unity.com/packages/tools/utilities/smart-logs-186319), records from the beginning all the way through where you wished to stop. Configurable. Was used on multiple mobile devices. Found to be pretty stable. Once a player is registered, you could filter based on inputs

### Mobile device testing

- Built internal device farm using selenium with phones plugged into a windows machine. Farm goes down very frequently and is a battle to keep those up. Recommend outsourcing

- [BrowserStack](https://www.browserstack.com/) for a virtual device farm for native and web based testing
  
  - [AWS Device Farm | Mobile and Web App Testing | Amazon Web Services](https://aws.amazon.com/device-farm/) as an alternative, but expensive and not as useful
  
  - Nowhere as good as a physical device, but better than maintaining all those specific devices

- Console device farms?
  
  - Internal server farm with 40 devkits, issue is that they're bound to a target machine. Not very scalable, all devices on-site.

### VR Testing device support

- Referenced in `Independent Games Summit: Fixing Bugs by Cloning them in 'The Last Clockwinder'`

### Choosing which tests to be automated

- [Bazel](https://bazel.build/) has a powerful dependency tool to help spot what requires to be tested, manually requires what dependencies are needed for testing 
  
  - Built in query language available at the analysis phase
  
  - There is a tool on the Xbox side which does something similar

- Internal Machine Learning tool that is trained by specifying categories and entities which works off every commit to determine which tests need to run

- Dependency graph to determine an owner of the dependency. Using tree iteration to find the owner using query language for reviews and maintenance

- Code coverage to determine which tests to run based on changes

### Build server capacity

- Build farm device pools set aside for commits, only run on selective pools to mitigate capacity
  
  - Cloud pool to handle general builds
  
  - On-prem to handle more stressful and resource intensive builds

- Using Azure to help scale-up the pool during work hours allows for machines to be ready by using trends
