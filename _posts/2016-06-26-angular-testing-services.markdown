---
layout: post
title: "Angular Testing: Services"
date: 2016-08-01 22:11
comments: true
categories: web-development
published: true
---
[Angular](https://angularjs.org/) [services](https://docs.angularjs.org/guide/services) serve as a way for us to store data that will be shared through our various Angular components. Services act as a single-point of truth for our application's data, and as such they are often reused, sometimes heavily. Ensuring services are well-tested is a crucial part of maintaining a healthy Angular app.

<!-- more -->

Along with acting as a data store, Angular components are singletons, which make them great candidates for housing shared code. In particular, services are a great interface between the Angular app and the backend, so, hopefully, your services are using Angular's `$http` service to do this communication. Mocking out the response from `$http` will be necessary to testing many services.

### Service Testing ###

Let's build our spec using the lessons we learned last time. We'll start with a `PlayerService`.

``` coffeescript
angular.module('core')
.factory 'PlayerService', ->

  getTypes: ->
    ['all', 'available']
```

Here is our Player service. Note that we are using a factory under the hood. The differences between factories and services are minimal, so it's usually best to pick one for a project and stick with it throughout.

``` coffeescript
describe 'PlayerService', ->
  PlayerService = null

  beforeEach ->
    module 'core'
    inject (_PlayerService_) ->
      PlayerService = _PlayerService_
```

And here's our spec for the Player service. Recalling a lesson from last time, we initialize the `PlayerService` at the top of the spec. In the `beforeEach` block, we initialize the `core` module, where our `PlayerService` lives. And then, because we intend on calling the service outside of this `beforeEach` block, we make sure we inject it using the underscore notation.

Let's add a spec for the `#getTypes` method.

``` coffeescript
describe 'PlayerService', ->
  PlayerService = null

  beforeEach ->
    module 'core'
    inject (_PlayerService_) ->
      PlayerService = _PlayerService_

  describe '#getTypes', ->
    it 'should return both types', ->
      expect(PlayerService.getTypes()).to.eql ['all', 'available']
```

Not too bad. Services are perfect candidates for unit testing. Every public method in a service can and should have a corresponding test.

In this instance, we don't need any further setup to test this method. We can make the call directly in the first argument of the `expect` call. Because the method returns an array, we need to use the [Chai](http://www.chaijs.com) method `eql`. `eq` can be used for primitives; it checks for direct equality. `eql` iterates through the items being compared and checks for deep equality. For this reason, `eql` must be used for checking equality of objects and arrays.

Now let's see an example where we need to test some more logic.

``` coffeescript
angular.module('core')
.factory 'PlayerService', (currentUser) ->

  getTypes: ->
    if currentUser.isAdmin
      ['all', 'available', 'pending']
    else
      ['all', 'available']
```

Now `#getTypes` checks on the value of an external service, `currentUser`, before returning a value. Let's see how to account for that external service and the extra conditional.

``` coffeescript
describe 'PlayerService', ->
  PlayerService = currentUser = scope = null

  beforeEach ->
    module 'core'
    inject (_PlayerService_, _currentUser_, $rootScope) ->
      PlayerService = _PlayerService_
      currentUser = _currentUser_
      scope = $rootScope.$new()

  describe '#getTypes', ->
    describe 'current user is admin', ->
      beforeEach ->
        currentUser.isAdmin = true
        scope.$digest()
      it 'should return both types', ->
        expect(PlayerService.getTypes()).to.eql ['all', 'available', 'pending']
    describe 'current user is NOT an admin', ->
      beforeEach ->
        currentUser.isAdmin = false
        scope.$digest()
      it 'should return both types', ->
        expect(PlayerService.getTypes()).to.eql ['all', 'available']
```

Alright, our spec has blown out a bit. We are injecting two more services. The first makes sense: we need to set values on `currentUser` to fully test out `#getTypes`. The second is a bit less intuitive: We inject `$rootScope` so we can create a scope for our test set up. We'll need this scope to run a digest cycle to update the state for our assertion.

We still have a single describe block for our single function, but now, we test out both forks of the conditional in our `#getTypes` method. So, we need test set up for each block. We set `isAdmin` on the external service before each expectation. After any sort of test set up, we need to digest the scope to propagate the change. And our method is fully tested!

### $http and $httpBackend ###

What will the service look like when we have an external service? We need to add the `$http` [service](https://docs.angularjs.org/api/ng/service/$http) to make a server-side request.

``` coffeescript
angular.module('core')
.factory 'PlayerService', ($http, $log) ->

  findAll: ->
    $http.get('/api/v1/players').then (response) ->
      response.data
    , (errors) ->
      $log(error) for error in errors
```

In this example, we're making a GET request. The `get` functions makes that request and returns a promise. The first argument in that promise is run on a successful request; the second is run on an unsuccessful one.

ngMock comes with `$httpBackend`: [a mocked out](https://docs.angularjs.org/api/ngMock/service/$httpBackend) `$http` service. Before we take a look at any expectations, let's take a look at the test set up.

``` coffeescript
describe 'PlayerService', ->
  PlayerService = currentUser = $httpBackend = null

  beforeEach ->
    module 'core'
    inject (_PlayerService_, _$httpBackend_) ->
      PlayerService = _PlayerService_
      $httpBackend = _$httpBackend_

  afterEach ->
    $httpBackend.verifyNoOutstandingExpectation()
    $httpBackend.verifyNoOutstandingRequest()
```

We'll be using $httpBackend throughout the spec, so we start by initializing it at the top and injecting it with the underscore syntax. Then, we immediately set up an `afterEach` block. Calling `#verifyNoOutstandingExpectation` and `#verifyNoOutstandingRequest` ensures that the expectations set up are here are torn down correctly. Without these calls, it's possible that the tests here could step on subsequent tests and cause them to fail. These test failures can be hard to hunt down and fix, so tearing down the `$httpBackend` expectations are imperative.

Let's look at a spec for the successful case next.

``` coffeescript
describe 'PlayerService', ->
  PlayerService = currentUser = null

  beforeEach ->
    module 'core'
    inject (_PlayerService_, _$httpBackend_) ->
      PlayerService = _PlayerService_
      $httpBackend = _$httpBackend_

  afterEach ->
    $httpBackend.verifyNoOutstandingExpectation()
    $httpBackend.verifyNoOutstandingRequest()

  describe '#findAll', ->
    describe 'successful', ->
      beforeEach ->
        $httpBackend.expectGET('/api/v1/players')
        PlayerService.findAll()
      it 'should return both types', ->
        $httpBackend.flush()
```

Let's walk through this one line by line starting with the test set up for `#findAll`. We start by calling `$httpBackend.expectGET` which sets up an expectation that the url passed in will be called. Next, we make the call to the method we are testing. Finally, in the `it` block, we call `flush`. This resolves all `$http` calls. With this flush, all of our expectations are met, and the test passes.

And our spec testing errors:

``` coffeescript
describe 'PlayerService', ->
  PlayerService = currentUser = $log = null

  beforeEach ->
    module 'core'
    inject (_PlayerService_, _$httpBackend_, _$log_) ->
      PlayerService = _PlayerService_
      $httpBackend = _$httpBackend_
      $log = _$log_

  describe '#findAll', ->
    describe 'successful', ->
      beforeEach ->
        $httpBackend.expectGET('/api/v1/players')
        PlayerService.findAll()
      it 'should return both types', ->
        $httpBackend.flush()
    describe 'error', ->
      beforeEach ->
        $httpBackend.whenGET('/api/v1/players').respond 422,
          ['Error']
        PlayerService.findAll()
        $httpBackend.flush()
      it 'should return both types', ->
        expect($log.debug.logs).to.eql ['Error']
```

`$httpBackend` has two different flavors of mocks. We saw the first in the success case: expect. The `expect` methods stub out requests that need to be called for the test to pass. In the error case we see the other flavor: when. The `when` methods are more forgiving. These requests don't need to be called, but when we flush out the `$http` service, we can set up the responses that will be returned. In fact, we'll get a "No pending requests to flush" error if we try to flush our requests that have not yet been set up. In this instance, we want to mock out an error, so we tell the service to return a 422 when it's flushed out. The second argument is the data response itself. The `$log` service holds the logs in the `debug` object which is set up perfectly for testing.

### Summary ###

Services are shared components that can be used throughout your application. Because so much of your application depends on them, services should be tested thoroughly. If you are communicating with a server, you should be setting up those HTTP calls with Angular's `$http` service. `$httpBackend` is set up to mock out these backend responses. Hopefully, these patterns and tips should give you a strong foundation for keeping all of your service tests strong.
