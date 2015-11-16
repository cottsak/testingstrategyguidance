# XYZ Testing Strategy

This outline briefly describes how we write automated tests for our production applications. It includes some theory, explanation and ultimately concrete examples with code. Please own this document and apply agile principals to inspect and adapt it regularly. Maintaining this document will serve as a point of reference so that everyone on the team is speaking the same “testing language”. It also serves to onboard new team members quickly.

## Why do we write tests for our code?

> hWe’re not here to write tests. We’re here to deliver value.

“Deliver value” here means deliver product value. This roughly translates to writing Production Code, and not tests, where Production Code is only the minimum logic required to serve the product delivering value to its customer.

## So why write any tests at all?

Having automated tests that you can run at the click of a button serves to give the developer and her team confidence that the Production Code is doing what it’s supposed to be doing: adding value to the product. So given a lean/agile approach, we want to write as few tests as possible to give the greatest return in terms of confidence.

If you don’t need confidence about a particular thing, don’t write a test for it. All code is mass and the more mass the greater the cost of maintaining said mass. This applies to Production Code and test code. Don’t treat test code as a second rate citizen.

## What types of should test should we write and in what quantities?

Some pundits believe there is a “ratio” of different types of tests. Some believe in the [testing pyramid](http://martinfowler.com/bliki/TestPyramid.html). Here are some types of tests and their pros and cons:

* Unit Tests – test only one thing: a "unit".  
Pros: extremely explicit; usually very fast  
Cons: tests only one thing; you need heaps of these; lots of tests/LOC for small relative gain/confidence; very isolated – no knowledge of connected parts
* Integration/Database Tests – these usually connect more than one call in the stack and integrate against a real or (almost real) database.  
Pros: covers more code; can still be fairly quick; you can usually write fewer for the same relative confidence as many more Unit Tests  
Cons: more moving parts == fragile; more complex; heavier to write / greater dev investment
* UI Automation / Functional Tests – usually these tests impersonate the end user directly under very production-like circumstances.  
Pros: capture the full stack of the application and other external environment factors; deliver huge ROI covering many LOC  
Cons: can be expensive to write, maintain, complex and in some cases brittle; usually there is a complex set up of the environment to make these work; often slow

Often folks are quoted as saying that you should have fewer heaver/slower/high-value tests like UI automation tests and greater qualities of smaller/faster/low-value tests like Unit Tests. We don’t believe in this guidance since is rigid and complicated.

## So just give me confidence; where is the silver bullet?

There is no silver bullet of course. However there is a type of test that can often strike a balance between investment and return in terms of initial, ongoing effort, and confidence. These are called [Subcutaneous tests](http://martinfowler.com/bliki/SubcutaneousTest.html). They sit logically just under the UI tests and substantially higher in the stack than the integration tests. In ASP.NET land they often target the “controller”.

Here is an example:

[image]

What you can’t see just from this image is that the test cuts a vertical slice though many layers of the system right down to the real database. A real entity is set up in the store. A HTTP request is made to delete the entity, and then a query is made against the store to verify that the entity no longer resides there. In this case we’re testing an API controller, a command, domain objects and persistence elements like ORM mapping and storage interaction. This test delivers a lot of confidence since it can be expressed in terms of a user story and ensures that nearly all pieces of the vertical slice are present and working for the ‘feature’ to be complete.

## So no Unit or UI tests then?

We don’t abstain from other types of tests, we simply use them when they give us more value than the confidence a subcutaneous (sub-c) test might provide from the controller level.

For example: a complex component that has many branching code paths may be more efficient to test at a unit level with perhaps data driven tests than multiple sub-c tests.

Another example: a UI component might make more sense to test with jasmine if it’s not making very many HTTP calls yet its in-browser behavior is very important.

We use our best judgement but are always trying to only invest the minimum effort required to allow us to be confident about the behavior we’re building.

## Well that example was pretty simple, we use Event Sourcing and Microservices; where is the code example for that?

TODO

Have Fun.

## Footnotes

Refer to this [Microtesting Presentation by Matt and Rob](https://github.com/MRCollective/MicrotestingPresentation) for a deeper dive into many of the concepts listed above.
