---
layout: post
post_author: Ryan Finke
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Testing Behavior vs. Testing Implementation
publish_date: 2014-04-03T13:40:00.000+00:00
feature_post_image: ''
post_images: []
slug: testing-behavior-vs-testing-implementation

---
Test-driven development is more art than science, and understanding what to test for comes only with experience. Further complicating things is that testing for the wrong stuff can create a suite of tests that are ugly, brittle, and provide false-positive passing tests. 

One of the keys to clean and reliable unit tests is the idea of testing for behavior vs. testing for implementation. The differences between these two characteristics are subtle, but really important to understand. Here is a simplified example showing this difference.

### When I test for behavior, I'm saying:
"I don't care how you come up with the answer, just make sure that the answer is correct under this set of circumstances"

### When I test for implementation, I'm saying:
"I don't care what the answer is, just make sure you do this thing while figuring it out."

In actual practice, what ends up happening is that a test looks for the result of the method (good), but it also makes assertions about how that answer was derived (not-so-good) by relying too much on mock and stubs. Its the mocks and stubs that get you into trouble as time passes and the system evolves.

## Testing implementation vs. Supporting implementation
I don't mean to imply that mocks and stubs are bad. They are incredibly useful in making good tests, and any meaningful test needs some knowledge of how the method actually works. Stubbing dependencies allows tests to run with different inputs to ensure code is in spec under different scenarios. This is supporting implementation. It turns bad when too many assertions start popping up on those stubs.

## An Exception
Sometimes, testing behavior and testing implementation are one in the same. This happens a lot in coordinator methods, whose purpose is to make a decision, and then delegate control (to a different class or method) accordingly. Since the behavior of this method is delegation, the only way you can test it is to assert that the correct stub was called.

## Example
Lets use an example to tie all this together. Below is a method called FindTerminals, whose job is to find a list of computers in the database (via the TerminalService object), then delegate to the ContactTerminal method for each one to see if its online. Here is what the method looks like:

````csharp
void FindTerminals() {
    terminals = terminalService.FindAll();
    foreach (var terminal in terminals)
        ContactTerminal(terminal);
}  
````

And here is the test for this method. Its pretty straight-forward. The TerminalService object is given a list of 3 terminals to substitute for the FindAll method, then we test that each terminal is contacted by counting the number of times the ContactTerminal method is called. Here's that:

````csharp
var terminals = new List<Terminal> {
    new Terminal(), new Terminal(), new Terminal() };           
terminalServiceMock
    .Setup(f => f.FindAll())
    .Returns(terminals);
var activeTerminalLocatorMock =
    new Mock<ActiveTerminalLocator>(
        terminalServiceMock.Object,
        networkServiceFactoryMock.Object,
        backgroundWorkerFactory);

activeTerminalLocatorMock.Object
    .FindTerminals(null);

activeTerminalLocatorMock.Verify(
    f => f.ContactTerminal(It.IsAny<Terminal>()),
    Times.Exactly(3)
);
````

So all is well and good. We get terminals, we contact terminals, the test is passing, everyone is happy. Except that if a terminal is turned off, we find our selves waiting up to 30 seconds per to time out, which quickly adds up while the user is standing there waiting for this thing to happen. As a convenience, we're going to change this method to contact each terminal asynchronously so that all terminals are contacted at once: 

````csharp 
void FindTerminals() {
    terminals = terminalService.FindAll();
    foreach (var terminal in terminals) {
        using(var worker = workerFactory.CreateWorker(){
            worker.DoWork += 
                (o, ea) => ContactTerminal(terminal);
            worker.RunWorkerAsync();
         }
     }
 }
````

And the test continues to pass! A fake worker factory is already being injected into the class for the method to use, and I don't intend to make assertions with it. The purpose of this method is not to do something asynchronously, but to contact each of the given terminals. That goal is reached regardless of the contacts being made inline or not - its just an implementation detail.

So here's what I can take away from this example:

* The implementation has changed from running code inline to running it async
* The behavior (or result of the method) has not changed
* The test has not changed

This is a good test. If a change in implementation triggers significant changes to a test (without a corresponding change in behavior), then there is a good chance the test is not providing a lot of value.