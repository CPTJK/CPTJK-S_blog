# Navigation Guards

## brief

There are a number of ways to hook into the route navigation process: globally, pre-route, or in-component.

Remember that **params or query changes won't trigger enter/leave navigation guards.** You can either watch the `$route` object to react to those changes , or use the `beforeRouterUpdate` in-component guard .

## global before Guards

You can register global before guards using `router.beforeEach`:

```JavaScript 
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

Global before guards are called in creation order , whenever a navigation is triggered. Guards may be resolved asynchronously, and the navigation is considered pending before all hooks have been resolved .

Every guard function receives three arguments:

* `to: Route`: the target Route object being navigated to.
* `from: Route`: the current Route object being navigated away from.
* `next: Function`: this function must be called to resolve the hook . The action depends on the  arguments provided to `next`:
  * `next()`: move to the next hook in the pipeline . If no hooks are left , the navigation is confirmed
  * `next(false)`: abort the current navigation . If the browser URL was changed (either manually by the user or via the back button ), it will be reset to that of the `from` Route.
  * `next('/')` or `next({path: '/'})`: redirect to a different location . The current navigation will be aborted and a new one will be started . You can pass any location object to `next`, which allows you to specify options like `replace: true`,`name; 'home'` and any option used in `route-link`'s `to` prop or `router.push`.
  * `next(error)`: if the argument passed to `next` is an instance of `Error`, the navigation will be aborted and the error will be passed to callbacks register via `router.onError`

**Make sure that the `next` function is called exactly once in any given pass through the navigation guard . It can appear more than once , but only if the logical paths have no overlap , otherwise the hook will never be resolved or produce errors .** Here is an example of redirecting user to `/login` if they are not authenticated:

```JavaScript 
// BAD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  // if the user is not authenticated, `next` is called twice
  next()
})
```

```JavaScript
// GOOD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
```

## Global Resolve Guards

You can register a global guard with `router.beforeResolve`. This is similar to `router.beforeEach`, with the difference that resolve guard will be called right before the navigation is resolved , **after all in-component guard and async route components are resolved**.

## Global After hooks

You can register global after hooks, however unlike guard , these hooks do not get a `next` function and can not affect the navigation :

```JavaScript
router.afterEach((to, from) => {
  // ...
})
```

## per-route guard

You can define `beforeEnter` guard directly on a route's configuration object :

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

These guards have the exact same signature as global before guards:

## in-component guards

Finally , you can directly define route navigation guards inside route components (the ones passed to the router configuration ) with the following options:

* beforeRouteEnter
* beforeRouteUpdate
* beforeRouteLeave

```JavaScript
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // called before the route that renders this component is confirmed.
    // does NOT have access to `this` component instance,
    // because it has not been created yet when this guard is called!
  },
  beforeRouteUpdate (to, from, next) {
    // called when the route that renders this component has changed.
    // This component being reused (by using an explicit `key`) in the new route or not doesn't change anything.
    // For example, for a route with dynamic params `/foo/:id`, when we
    // navigate between `/foo/1` and `/foo/2`, the same `Foo` component instance
    // will be reused (unless you provided a `key` to `<router-view>`), and this hook will be called when that happens.
    // has access to `this` component instance.
  },
  beforeRouteLeave (to, from, next) {
    // called when the route that renders this component is about to
    // be navigated away from.
    // has access to `this` component instance.
  }
}
```

The `beforeRouteEnter` guard does **not** have access to `this` , because the guard is called before the navigation is confirmed , thus the new entering component has not even been created yet .

However , you can access the instance by passionate a callback to `next`. The callback will be called when the navigation is confirmed, and the component instance will be passed to the callback as the argument :

```JavaScript
beforeRouteEnter (to, from, next) {
  next(vm => {
    // access to component instance via `vm`
  })
}
```

Note that `beforeRouteEnter` is the only guard that supports passing a callback to `next`. For `beforeRouteUpdate` and `beforeRouteLeave`, `this` is already available , so passing a callback is unnecessary and therefore not supported:

```JavaScript
beforeRouteUpdate (to, from, next) {
  // just use `this`
  this.name = to.params.name
  next()
}
```

The leave guard is usually used to prevent the user from accidentally leaving the route with unsaved edits. The navigation can be cancelled by calling `next(false)`.

```JavaScript
beforeRouteLeave (to, from, next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```

If you are using mixins that add in-component navigation guards , make sure to add the mixin after installing the router plugin :

```JavaScript
Vue.use(Router)

Vue.mixin({
  beforeRouteUpdate(to, from ,next) {
    // ...
  }
})
```

## The full navigation resolution flow

1. navigation triggered
2. call `beforeRouteLeave` guards in deactivated components
3. call global `beforeEach` guards
4. call `beforeRouteUpdate` guards in reused components
5. call `beforeEnter` in route configs
6. resolve async route components
7. call `beforeRouteEnter` in activated components
8. call global `beforeResolve` guards
9. navigation confirmed
10. call global `afterEach` hooks
11. DOM updates triggered
12. call callbacks passed to `next` in `beforeRouteEnter` guards with instantiated instances.
