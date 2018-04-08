---
layout: post
title: "An Introduction to Angular Testing"
date: 2016-05-17 13:46
comments: true
categories: web-development
published: false
---
[AngularJS](https://angularjs.org/) is designed to be as testable as possible. With its use of dependency injection, creating and manipulating mocks to test out individual components of your Angular application can be done in a stable, reliable way. Testing out each Angular component, however, comes with its own set of patterns to apply. Some of these patterns are similar cross-component, and using and understanding these core concepts of Angular testing can act as a nice bootstrap to your testing process.

<!-- more -->

There are a number of tools that can be combined to create an Angular framework. For these purposes, I'll be using [CoffeeScript](http://coffeescript.org/) as a JavaScript precompiler, [Karma](https://karma-runner.github.io/) as a test runner, [Mocha](https://mochajs.org/) as a testing library, and [Chai](http://chaijs.com/), with expect syntax, as an assertion library. Before we take a look at testing Angular components, let's start by getting a closer look at our testing and assertion libraries.


### Mocha and Chai ###

Let's start by taking a look at a very pared down of an AngularJS test, and we'll build on it over the course of this post.

``` coffeescript
describe 'TeamService', ->
  describe 'exists', ->
    ...
```

If you've used a test framework like RSpec or Minitest for Ruby before, the structure should look familiar. Mocha, and Jasmine, organize test assertions inside `describe` blocks. `describe` methods take two arguments: a string that should act as a human readable description for the state that follows, and a method that will contain a mix of assertions and more `describe` methods. These `describe` blocks do two things: they group specs together to help create a readable assertion and they provide a place to set up mocks for each assertion.

Let's see how we can use describe blocks to set up different test scenarios using Mocha's beforeEach method.

``` coffeescript
describe 'TeamService', ->
  TeamService = null

  describe 'when a team is passed in', ->
    beforeEach ->
      TeamService.team =
        id: 1
    it 'should should have a team', ->
      expect(TeamService).to.have.property('team')
  describe 'when a team is not passed in', ->
    it 'should not have a team', ->
      expect(TeamService).to.not.have.property('team')
```

There's a lot going on above, so let's take it one thing at a time. Let's focus on the `beforeEach` method added above. By organizing our Angular specs into `describe` blocks that connote state, we're able to set up test data inside of the `beforeEach` method. `beforeEach` takes one argument: a method that will be executed before assertions are run. In fact, we're able to continue to nest `describe` blocks with their own `beforeEach` method, and we can be confident that they will run in order before each assertion is run. This allows us to construct specs that build state on themselves which should lead to readable, DRY tests. In the above example we describe two different states, and we're able to set up each using `beforeEach`.

In order to access these variables across various `describe` blocks, we declare these variables inside the outermost `describe` block. We set our CoffeeScript variables equal to `null` as a way of declaring them. We could just as easily declare these by setting them equal to `undefined`. There should be no difference, so pick a convention and stick with it.

One last thing to note on the example above: we've added some new types of assertions. Assertions are denoted by the `it` method, which takes a string and a method. The second argument of the `it` block should contain an assertion. These assertions are the heart of the spec. As stated above, this assertion is written using Chai as our assertion library for our foray into Angular testing. Chai offers a bevy of different, interesting, and, most importantly, descriptive ways to write assertions. In the above example, our first assertions ensure the `team` property is on `TeamService`, and in the second, we assert that it's not.

### Angular ###

In order to more easily test our Angular code, the Angular team provides a mocking library, ngMock, a mocking library. ngMock has two very important methods: `module` and `inject`. `module` take a string and loads in the corresponding Angular module. `inject` takes functions that wrap injectable services and providers. Let's see them in action:

``` coffeescript
describe 'TeamService', ->
  TeamService = null

  beforeEach ->
    module 'core'
    inject (_TeamService_) ->
      TeamService = _TeamService_
      TeamService.emptyRoster()
  it 'should have an empty list of players', ->
    expect(TeamService.players).to.eql []
```

The very first thing we do inside our root `beforeEach` block is load the module that the `TeamService` is on. As soon as its loaded, we can begin injecting any dependencies we need to call as a part of testing the `TeamService`. In this case, we inject the `TeamService`. One thing to note is the underscore wrapping notation we use to inject the service. If we were to inject `TeamService`, we would not be able to reference the service in any subsequent `describe` or `it` blocks without declaring it in a parent scope. If we declare it at the top as `TeamService` however, then, we won't be able to inject `TeamService`. In order to handle this, Angular mocks provide us with an underscore-wrapped version of every service which resolves to that service itself. Injecting `_TeamService_` and setting it to the `TeamService` declared above, will then allow us to use `TeamService` throughout the spec. As a rule of thumb, if you only need to use a service inside the top-level `describe` block, you can use the standard service injection syntax; if you will need to call that service in subsequent blocks of the spec, you should use the underscore syntax for injection.

Once the services and providers needed for the spec are declared and injected, we can use them throughout our test. In this case, we confirm that after running `TeamService.emptyRoster()` the `TeamService` has an empty `players` array.

### Summary ###

Mochai and Chai can be great building blocks to setting up JS and Angular specs. With the patterns above, we are just scratching the surface. In future posts, I'll explore more patterns for testing Angular Services, Controllers, Directives, and more. So check back soon!
