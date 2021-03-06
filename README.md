## AngularUI Router Breakout

Up until now we've been using the ngRoute module as our routing service. This has a lot of limitations as you might have already seen. It's the simplest way in Angular to handle client-side routing.

You probably have found yourself using `ng-hide` and `ng-show` to mimic the functionality not provided to you with ngRoute, such as nested views. Here we'll dive into the advantages of using a router that supports routing based on state and behavior changes, as well as, nested views. A brief example can be found [here](http://angular-ui.github.io/ui-router/sample/#/).

Just like with ngRoute, we have to bring in the external library for ui-router. This can be done with link to a CDN or with a package manager such as `npm install angular-ui-router`. Here is an example script tag with the CDN already linked.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/angular-ui-router/0.3.1/angular-ui-router.min.js"></script>
```

Use the generator and run `yo galvanize-html`. If you haven't updated this generator in a while, first run `npm i -g generator-galvanize-html` to grab any new changes. Don't forget to run `npm i` and install the dependencies. We're going to use the following directory structure for components:
```
├── components
│   ├── about
│   │   ├── about.controller.js
│   │   ├── about.view.html
│   │   └── partials
│   │       └── _contact.html
│   └── main
│       ├── main.controller.js
│       ├── main.view.html
│       └── partials
│           └── _list.html
```
Create a new folder inside components for our `about` view. Add a controller and a view for this component. Here is very simple example of a possible component view.

```html
<h1>Contacts</h1>
<hr/>
<a ui-sref="about.contacts">Show Contact!</a>
<div ui-view></div>
```

### ngRoute vs uiRouter
With the ngRoute module, we used the ng-view directive, `<ng-view></ng-view>`, so that the templates know where to get "hot-swapped". We do the same with ui-router except the directive name is `<ui-view></ui-view>`. In the body of index.html, add

```html
<div class="container">
  <a ui-sref="about">About</a> |
  <a ui-sref="home">Home</a>
  <div ui-view></div>
</div>
```

### $stateProvider && $urlRouterProvider
The service we used with ngRoute was called `$routeProvider`. With uiRouter, we'll use the `$stateProvider` and `$urlRouterProvider` services. Instead of calling the `.when()` method, where we gave it the route and a object defining the controller and template, we'll use the `.state()` method which works very similarly. Here is the snippet from the code example included in this repo.


```js
function appConfig($stateProvider, $urlRouterProvider) {
  $urlRouterProvider.otherwise('/');
  $stateProvider
    .state('home', {
      url: '/',
      templateUrl: 'js/components/main/main.view.html'
    })
    .state('home.list', {
      templateUrl: 'js/components/main/partials/_list.html',
      controller: 'mainController',
      controllerAs: 'mainCtrl'
    })
    .state('about', {
      url: '/about',
      templateUrl: 'js/components/about/about.view.html'
    })
    .state('about.contacts', {
      templateUrl: 'js/components/about/partials/_contact.html',
      controller: 'aboutController',
      controllerAs: 'aboutCtrl'
    });
}
```

In the example above, we have the home route and about route, each with a nested route. You'll notice when we click the Show List! button, our route doesn't change, but the state does via our `ui-sref` attribute. The `$urlRouterProvider` service exposes a method called otherwise, just like `$routeProvider` method of the same name. This allows to specify a default for our client-side routing, in case the end-user some how has ended up at a route that doesn't exist.

### Create some views!
Now we are going to add "partials" or child templates of another html page. Let's create a partials directory inside a component and add another `.html` file. A common naming convention for partials you'll probably see is to prefix the file name with an underscore such as `_list.html`. Create a partial which we will use as a nested view. Here is an example partial for the home/main route:

```html
<h3>Home List!</h3>
<ul>
  <li ng-repeat="thing in mainCtrl.things">{{ thing }}</li>
</ul>
```

This template will get hotswapped into our main.view.html, not index.html. This is the advantage of nested templates. Make another view for our about component to use. Here's an example:

```html
<h1>Contacts</h1>
<hr/>
<a ui-sref="about.contacts">Show Contact!</a>
<div ui-view></div>
```

Once again, clicking the Show Contact button changes our "state" with the `ui-sref` attribute, but doesn't affect the actual route/endpoint you see in the browser. Let's create a partial to get rendered inside this template. Here's an example:

```html
<h3>Home List!</h3>
<ul>
  <li ng-repeat="thing in mainCtrl.things">{{ thing }}</li>
</ul>
```
