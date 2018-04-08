---
layout: post
title: "AngularUI Router and Injecting $state vs $stateParams"
date: 2016-01-31 19:31
comments: true
categories: web-development
published: true
---
AngularUI Router is the de facto routing library in the Angular world. It takes the traditional routing mechanisms, and builds a subtle, but brilliant abstraction upon them. Instead of merely listening for requests at a set of URLs, it creates the concept of a set of states, each one configurable with an optional URL. This abstraction allows for flexibility when refactoring routes, but most interestingly, it creates the concept of a current state and stores key-value parameters of that state. Both the application's state and state parameters are available for injection with $state and $stateParams respectively, although, as we'll see, only one is necessary for injection in any given controller.

<!-- more -->

The `$state` service provides a number of useful methods for manipulating the state as well as pertinent data on the current state. The current state parameters are accessible on the $state service at the `params` key. The `$stateParams` service returns this very same object. Hence, the `$stateParams` service is strictly a convenience service to quickly access the `params` object on the `$state` service.

``` javascript
angular.equals($state.params, $stateParams)
// true
```

As such, no controller should ever inject both the `$state` service and its convenience service, `$stateParams`. If the `$state` is being injected just to access the current parameters, the controller should be rewritten to inject `$stateParams` instead.

Let's take a look at a few examples to could be rewritten to minimize our injections and concerns and to generally clean up our controllers.

**Example: Only need to access state parameters**

_Before:_
``` javascript
MainCtrl = (function() {
  function MainCtrl($state) {
    this.label = $state.params.active;
  }
})();
app.controller('MainCtrl', ['$state', MainCtrl]);
```

_After:_
``` javascript
MainCtrl = (function() {
  function MainCtrl($stateParams) {
    this.label = $stateParams.active;
  }
})();
app.controller('MainCtrl', ['$stateParams', MainCtrl]);
```

In the above example, the controller only needs to access the parameters of the current state. Injecting the `$state` service is, therefore, unnecessary. Injective the convenience service here is preferred.

**Example: Need to both transition state and access params**

_Before:_
``` javascript
MainCtrl = (function() {
  function MainCtrl($state, $stateParams) {
    this.label = $stateParams.active;
  }

  MainCtrl.prototype.goToBeta = function() {
    this.$state.go('beta');
  }
})();
app.controller('MainCtrl', ['$state', '$stateParams', MainCtrl]);
```

_After:_
``` javascript
MainCtrl = (function() {
  function MainCtrl($state) {
    this.label = $state.params.active;
  }

  MainCtrl.prototype.goToBeta = function() {
    this.$state.go('beta');
  }
})();
app.controller('MainCtrl', ['$state', MainCtrl]);
```

In this example, we are using the `$state` service for its methods and not just for the parameters data it holds. In this case, again, we see that we do not need to inject both services. We can stick with the `$state` service and merely access the `params` key on the service.

As your controllers increase in complexity, so, too, do the benefits of including one service or the other. The list of injected services acts as a signature for a controller. Keeping this list lean is imperative to ensuring the controller's purpose is communicated efficiently. Ensuring that only one of the $state and $stateParams services will only help your projects' overall readability.
