# preact-router

[![NPM](http://img.shields.io/npm/v/preact-router.svg)](https://www.npmjs.com/package/preact-router)
[![travis-ci](https://travis-ci.org/developit/preact-router.svg)](https://travis-ci.org/developit/preact-router)

Connect your [Preact] components up to that address bar.

`preact-router` provides a `<Router />` component that conditionally renders its children when the URL matches their `path`. It also automatically wires up `<a />` elements to the router.

> 💁 **Note:** This is not a preact-compatible version of React Router. `preact-router` is a simple URL wiring and does no orchestration for you.
>
> If you're looking for more complex solutions like nested routes and view composition, [react-router](https://github.com/ReactTraining/react-router) works great with preact as long as you alias in [preact-compat](https://github.com/developit/preact-compat).  React Router 4 even [works directly with Preact](http://codepen.io/developit/pen/BWxepY?editors=0010), no compatibility layer needed!

#### [See a Real-world Example :arrow_right:](http://jsfiddle.net/developit/qc73v9va/)

---


### Usage Example

```js
import Router from 'preact-router';
import Terms from './Components/Terms/Terms.jsx';
import { h, render } from 'preact';
import AsyncRoute from 'preact-router/async';
/** @jsx h */

function getProfile(){
	return System.import('../component/Profile/Profile.jsx').then(module => module.default);
}

function getFriendsComponent(url, cb){
	System.import('../component/Profile/Profile.jsx').then(module => {
		cb(null, module.default);
	});
}

const Main = () => (
	<Router>
		<Home path="/" />
		<About path="/about" />
		<Search path="/search/:query" />
		<Route path="/terms" component={Terms} />
		<AsyncRoute path="/profile/:userid" component={Profile} />
		<AsyncRoute path="/friends/:userid" component={getFriendsComponent} />
	</Router>
);

render(<Main />, document.body);
```

If there is an error rendering the destination route, a 404 will be displayed.

### Handling URLS

:information_desk_person: Pages are just regular components that get mounted when you navigate to a certain URL.
Any URL parameters get passed to the component as `props`.

Defining what component(s) to load for a given URL is easy and declarative.
You can even mix-and-match URL parameters and normal `props`.

```js
<Router>
  <A path="/" />
  <B path="/b" id="42" />
  <C path="/c/:id" />
  <D default />
</Router>
```

### Lazy Loading

Lazy loading (code splitting) with `preact-router` can be implemented easily using the [AsyncRoute](https://www.npmjs.com/package/preact-async-route) module:

```js
import AsyncRoute from 'preact-async-route';
<Router>
  <Home path="/" />
  <AsyncRoute
    path="/friends"
    component={ () => import('./friends') }
  />
  <AsyncRoute
    path="/friends/:id"
    component={ () => import('./friend') }
    loading={ () => <div>loading...</div> }
  />
</Router>
```

---


### License

[MIT]


[Preact]: https://github.com/developit/preact
[MIT]: http://choosealicense.com/licenses/mit/
