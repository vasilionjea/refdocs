# Container & Registry
Every Container instance is associated with a Registry instance.

```js
  // ------- Container & Registry ------------------------------------------ //
  //= A container has a registry
  let container = new Container(new Registry(...), options);
  console.log(container.registry) //=> a Registry instance


  // ------- Engine & EngineInstance --------------------------------------- //
  //= An Engine has its own registry
  let engine = Engine.create({
    __registry__: new Registry(...)
  });

  //= An EngineInstance has its own registry and container
  let engineInstance = EngineInstance.create({
    __registry__: new Registry(...)
  });
  // registry.container() simply returns `new Container(this, ...)`
  engineInstance.__container__ = registry.container({ owner: engineInstance });


  // ------- Application & ApplicationInstance ----------------------------- //
  //= Application extends from Engine
  const Application = Engine.extend({
    // and since it extends from Engine, it has its own __registry__
  });

  //= ApplicationInstance extends from EngineInstance
  const ApplicationInstance = EngineInstance.extend({
    // since it extends from EngineInstance, it has its own:
    // * __registry__
    // * __container__
  });

  // Global mode => window.App = Ember.Application.create();
  // Only in globals mode, the same container instance exists in both the app and appInstance instances
  // ApplicationInstance instances have a reference back to their `application`
  appInstance.__container__ === appInstance.application.__container__ //=> true
```

### Container
  Every Container instance is associated with a Registry instance: `container.registry //=> a Registry instance`
  ```js
  function Container(registry, options) {
    this.registry = registry;
    this.owner = options.owner || null;
    this.cache = /*...*/;
  }
  ```

### Registry
  A registry can create a new Container instance via its `container()` method.
  ```js
  function Registry(options) {
    //...
  }
  Registry.prototype = {
    // Creates a container based on this registry.
    container(options) {
      return new Container(this, options);
    }
  };
  ```

### Engine
  An `Engine` has its own registry:
  ```js
  /**
    The `Engine` class contains core functionality for both applications and engines.
    Each engine manages a registry that's used for dependency injection and exposed through `RegistryProxy`.
    Engines also manage initializers and instance initializers.
    Engines can spawn `EngineInstance` instances via `buildInstance()`.
    @extends Ember.Namespace
    @uses RegistryProxy
    @public
  */
  const Engine = Namespace.extend(RegistryProxyMixin, {
    init() {
      this._super(...arguments);
      this.buildRegistry();
    },

    /**
      Build and configure the registry for the current engine.
      @private
      @return {Ember.Registry} the configured registry
    */
    buildRegistry() {
      let registry = this.__registry__ = this.constructor.buildRegistry(this);
      return registry;
    },
  });

  Engine.reopenClass({
    buildRegistry(namespace, options = {}) {
      let registry = new Registry(...);
      //...here default injections are registered, e.g. routing service
      return registry;
    },
  });
  ```

### EngineInstance
  An `EngineInstance` has its own registry:
  ```js
  /**
    The `EngineInstance` encapsulates all of the stateful aspects of a running `Engine`.
  */
  const EngineInstance = EmberObject.extend(RegistryProxyMixin, ContainerProxyMixin, {
    /**
      The base `Engine` for which this is an instance.
      @property {Ember.Engine} engine
      @private
    */
    base: null,

    init() {
      //...
      let base = this.base;

      if (!base) {
        base = this.application;
        this.base = base;
      }

      // Create a per-instance registry that will use the application's registry
      // as a fallback for resolving registrations.
      let registry = this.__registry__ = new Registry({
        fallback: base.__registry__
      });

      // Create a per-instance container from the instance's registry
      this.__container__ = registry.container({ owner: this });
      //...
    },
  });
  ```

### Application
  ```js
  const Application = Engine.extend({
    // since it extends from Engine, it has its own `__registry__`
  });
  ```

### ApplicationInstance
  ```js
  const ApplicationInstance = EngineInstance.extend({
    // since it extends from EngineInstance, it has its own `__registry__`
    // since it extends from EngineInstance, it has its own `__container__`
  });
  ```
  
# Route handler lifecycle hooks
### Consider the following tree of handlers:
```
|~index ("/")
|  |~posts ("/posts")
|  |  |-showPost ("/:id")
|  |  |-newPost ("/new")
|  |  |-editPost ("/edit")
|  |~about ("/about/:id")
```

### Consider the following transitions:
1. A URL transition to _/posts/1_
  * Triggers the `beforeModel()`, `model()`, `afterModel()`, `redirect()` callbacks on the "index", "posts", and "showPost" handlers
  * Triggers the `enter` callback on the same handlers
    - `activate()`
  * Triggers the `setup` callback on the same handlers
  - `setupController()`
  - `renderTemplate()`

2. A direct transition to _newPost_
  * Triggers the `beforeModel()`, `model()`, `afterModel()`, `redirect()` callbacks on the "newPost".
  * Triggers the `exit()` callback on "showPost"
    - `deactivate()`
  * Triggers the `enter` callback on "newPost"
    - `activate()`
  * Triggers the `setup` callback on "newPost"
    - `setupController()`
    - `renderTemplate()`

### Resources
* https://github.com/tildeio/route-recognizer
* https://github.com/tildeio/router.js
* https://github.com/emberjs/ember.js/blob/master/packages/ember-routing/lib/system/dsl.js
* https://github.com/emberjs/ember.js/blob/master/packages/ember-routing/lib/system/router.js
* https://github.com/emberjs/ember.js/blob/v2.11.0/packages/ember-routing/lib/system/route.js

### 1. TransitionState https://github.com/tildeio/router.js/blob/816c11f99bb169300ec98dbdb613bc2c0cf6c175/lib/router/transition-state.js
States have handler infos: `state.handlerInfos`
```
function proceed(resolvedHandlerInfo) {
  ...
  // redirect() called here
  callHook(handler, 'redirect', resolvedHandlerInfo.context, payload)

  ...
  // Proceed after ensuring that the redirect hook
  // didn't abort this transition by transitioning elsewhere.
  return innerShouldContinue().then(resolveOneHandlerInfo, ...);
}

function resolveOneHandlerInfo() {
  ...
  var handlerInfo = currentState.handlerInfos[payload.resolveIndex];
  return handlerInfo.resolve(innerShouldContinue, payload).then(proceed, ...)
}

resolve {
  resolveOneHandlerInfo {
    proceed {
      ∞ resolveOneHandlerInfo ∞ proceed
    }
  }
}
```

### 2. HandlerInfo https://github.com/tildeio/router.js/blob/816c11f99bb169300ec98dbdb613bc2c0cf6c175/lib/router/handler-info.js
Handler infos have handlers: `handlerInfo.handler`. `handler` is an Ember.Route
```
resolve: function(shouldContinue, payload) {
  ...
  .then(checkForAbort, null, self.promiseLabel("Check for abort"))
    .then(beforeModel, null, self.promiseLabel("Before model"))
  .then(checkForAbort, null, self.promiseLabel("Check if aborted during 'beforeModel' hook"))
    .then(model, null, self.promiseLabel("Model"))
  .then(checkForAbort, null, self.promiseLabel("Check if aborted in 'model' hook"))
    .then(afterModel, null, self.promiseLabel("After model"))
  .then(checkForAbort, null, self.promiseLabel("Check if aborted in 'afterModel' hook"))
    .then(becomeResolved, null, self.promiseLabel("Become resolved"));
  ...
}
```

### 3. Router https://github.com/tildeio/router.js/blob/b4419b7531341c18e20f0e961714fc2a39d2b404/lib/router/router.js#L162
```
handleURL {
  doTransition {
    let intent = new URLTransitionIntent(...) || new NamedTransitionIntent(...)
    this.transitionByIntent(intent)
  }
}

transitionByIntent {
  ...
  getTransitionByIntent {
    var newState = intent.applyToState(oldState, ...) // a TransitionState instance
    var newTransition = new Transition(this, intent, newState) // on init-> newState.resolve(...)
    ...
    // newTransition.promise.then == newState.promise
    finalizeTransition {
      ...
      setupContexts {// runs all the necessary enter/setup/exit hooks
        callHook(handler, 'exit', transition); // 1. exit previous handlers
        ...

        handlerEnteredOrUpdated {
          ...
          callHook(handler, 'enter', transition); // 2. enter new handler
          ...
          callHook(handler, 'setup', context, transition); // 3. set up new handler
        }
      }
      ...

      updateURL {
        // Generate URL from handler using recognizer
        var url = router.recognizer.generate(handlerName, params);

        // Update URL in Browser
        this.router.updateURL(url); // this.router == Ember.Router
      }
    }
  }
}
```

### 4. Ember.Router https://github.com/emberjs/ember.js/blob/master/packages/ember-routing/lib/system/router.js#L635
```
// ApplicationInstance calls: router.startRouting()
startRouting {
  ...
  setupRouter {
    ...
    routerMicrolib.updateURL = function(path) {
      lastURL = path;
      ...
      // Uses `history.pushState` to update the url without a page reload.
      doUpdateURL {
        location.setURL(lastURL);
        set(emberRouter, 'currentURL', lastURL);
      }
    };
    ...
  }

  // Register a callback to be invoked whenever the browser
  // history changes, including using forward and back buttons.
  location.onUpdateURL(url => {
    routerMicrolib.handleURL(url || '/')
  });
}
```

