# <img src="https://cloud.githubusercontent.com/assets/7833470/10899314/63829980-8188-11e5-8cdd-4ded5bcb6e36.png" height="60"> Angular Directives and Expressions

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

One of Angular's central structures is the directive - additional markup we can add to our HTML.  Angular uses these directives when it pre-processes our HTML files with its **HTML compiler**, and it attaches behavior to a DOM element.

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

- Extend an app's functionality with Angular's built-in directives

### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

- Explain the benefits of using Angular and describe the problems Angular aims to solve.
- Initialize Angular in an HTML view, and use expressions and templates to impact the DOM.
- Organize an Angular app with front-end controllers and connect the View & Controller using `this`.
- Implement 2-way data binding.



## What are directives?

> At a high level, directives are markers on a DOM element (such as an attribute, element name, comment or CSS class) that tell AngularJS's **HTML compiler** (`$compile`) to attach a specified behavior to that DOM element (e.g. via event listeners), or even to transform the DOM element and its children. <a href="https://docs.angularjs.org/guide/directive#what-are-directives-" target="_blank">AngularJS Developer Guide</a>

In Angular, we **add behavior to HTML** by using directives. A directive is a marker on an HTML tag that tells Angular to run or reference JavaScript code.

Angular directives start with the prefix `ng-`. You've already seen a couple of built-in directives:

* `ng-app`: turns ordinary HTML into an Angular application.
* `ng-controller`: attaches a controller to the view (or a specific section of the view).

Two more that you will use often:

* `ng-model`: binds a form element's value to a property on `vm`.
* `ng-repeat`: iterates over a collection.

Here is a full <a href="https://docs.angularjs.org/api/ng/directive" target="_blank">list of Angular's built-in directives</a>.


### ngModel

Assume you have the following Angular app set up for a trainer to manage their Pokémon. Here is the HTML snippet containing your Angular app:

```html
<!-- index.html -->

<!DOCTYPE html>
<html ng-app="pokemonApp">
  <head>
	<!-- bootstrap -->
	<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
	<!-- angular -->
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.8/angular.js"></script>

	<!-- custom script (angular app) -->
	<script type="text/javascript" src="app.js"></script>

  </head>
  <body ng-controller="PokemonController as pokemon">

	  ...

  </body>
</html>
```

And here is the JavaScript code that initializes your Angular app and adds the collection of Pokémon to `vm`. `vm` represents the View Model and will be the standard attribute we will be placing data into:

```js
// app.js

angular
  .module('pokemonApp', [])
  .controller('PokemonController', PokemonController);

  function PokemonCtrl () {

  	var vm = this;

    vm.pokemonList = [
      {
        id: 25,
        name: 'Pikachu',
        type: 'Electric'
      },
      {
        id: 10,
        name: 'Caterpie',
        type: 'Bug'
      },
      {
        id: 39,
        name: 'Jigglypuff',
        type: 'Fairy'
      },
      {
        id: 94,
        name: 'Gengar',
        type: 'Ghost'
      },
      {
        id: 143,
        name: 'Snorlax',
        type: 'Normal'
      }
    ];
    console.log(vm);
  };

```

You want the user to be able to input their name in a form field so that the application acknowledges them as the trainer for these Pokémon. You can use the `ngModel` directive to bind the form input to another element on the page:

```html
<!-- index.html -->

<body ng-controller="PokemonController as pokemon">

  <h2>Trainer: {{pokemon.trainer.name}}</h2>

  <span>Enter your name:</span>
  <input type="text" ng-model="pokemon.trainer.name">

</body>
```

This "binds" the value of `pokemon.trainer.name` between the form input and the `h2` tag. Whenever the user types something in the form, the `h2` tag's text will automatically update.

Using `ngModel` with `pokemon.trainer.name` adds a `trainer` object to `vm`. If you were to inspect `vm` using the debugger, it would look something like this:

```js
vm= {
  trainer: {
    name: "Whatever the user typed in the form."
  }
}
```

What if you wanted to set a default value for the trainer's name so that when the page loads, it's set to "Ash"?

You can set a default value in the Angular templating parameters:


```html
<!-- index.html -->

<body ng-controller="PokemonController as pokemon">

  <h2>Trainer: {{pokemon.trainer.name || "Ash"}}</h2>

  <span>Enter your name:</span>
  <input type="text" ng-model="pokemon.trainer.name">

</body>
```

Or directly on `vm` in the controller:

```js
// app.js

angular
  .module('pokemonApp', [])
  .controller('PokemonCtrl', PokemonCtrl);

function PokemonCtrl () {

  var vm = this;

  vm.trainer = {
    name: "Ash"
  };

  vm.pokemonList = [
    {
      id: 25,
      name: 'Pikachu',
      type: 'Electric'
    },
	...
  ];
  console.log(vm);
};

```

### ngRepeat

You can use `ngRepeat` to iterate through the collection of Pokémon and display them on the page!

```html
<!-- index.html -->

<body ng-controller="PokemonCtrl as pokemon">

  <div ng-repeat="poke in pokemon.pokemonList">
    <p>{{poke.name}}</p>
  </div>

</body>
```

### Filtering Data

In addition to directives, Angular also has built-in  <a href="https://docs.angularjs.org/api/ng/filter" target="_blank">filter components</a> that format data for displaying it to the user.

You can use the <a href="https://docs.angularjs.org/api/ng/filter/orderBy" target="_blank">orderBy</a> filter to sort the Pokémon by id number:

```html
<!-- index.html -->

<body ng-controller="PokemonCtrl as pokemon">

  <div ng-repeat="poke in pokemon.pokemonList | orderBy:'id'">
    <p>{{poke.name}}</p>
  </div>

</body>
```

You can use the <a href="https://docs.angularjs.org/api/ng/filter/filter" target="_blank">filter</a> filter to allow users to search the list of Pokémon by typing any Pokémon attribute (name, id, etc.). Notice the use of `ng-model` to bind the value of the search field to the filter on the collection.

```html
<!-- index.html -->

<body ng-controller="PokemonCtrl as pokemon">

  <span>Search Pok&#233;mon:</span>
  <input type="text" ng-model="searchPokemon">

  <div ng-repeat="poke in pokemon.pokemonList | orderBy:'id' | filter:searchPokemon">
    <p>{{poke.name}}</p>
  </div>

</body>
```

To see one solution, check out the [solution](https://github.com/SF-WDI-LABS/angular_directives_lab/tree/solution) branch.

## Challenges

Work on the [Angular Movie Lab](https://github.com/SF-WDI-LABS/angular-movie-lab).

## Resources

* <a href="https://docs.angularjs.org/guide/directive#what-are-directives-" target="_blank">AngularJS Developer Guide - Directives</a>
* <a href="https://docs.angularjs.org/api/ng/directive" target="_blank">AngularJS API Reference - Directives</a>
* <a href="https://docs.angularjs.org/api/ng/filter" target="_blank">AngularJS API Reference - Filter Components</a>
