Directives
---

### dv-color
- Run `yo angular:directive dv-color` in your terminal. Yeoman will create a new directive for you named dv-color.
- Find some text in team.html and add `dv-color=”gold”` as an attribute to the HTML element. Mine looks like this...

```
<h1>{{ team.name }}</h1>

<ul>
    <li ng-repeat="character in team.characters" dv-color="gold">
        {{ character.name }}
    </li>
</ul>

<hr>
<a ui-sref="main">Back To Main</a>
```

- Open `app/scripts/directives/dv-color.js`
- Remove `template: ‘<div></div>’`
- Change `restrict: ‘E’` to `restrict: ‘A’`
- Edit the link function to pull the `dvColor` attribute off of the `attrs` argument. Use the `element` argument to set your color attribute using jQuery’s .css() function.

The finished product should look like this...

```
'use strict';

angular.module('tmntApp')
  .directive('dvColor', function () {
    return {
      restrict: 'A',
      link: function postLink(scope, element, attrs) {
        element.css('color', attrs.dvColor);
      }
    };
  });
```

- Enjoy the golden character names!

### dv-append
- Run `yo angular:directive dv-append`. Yeoman will create a new directive for you named dv-append.
- Open `team.html` and add a new element named `<dv-append></dv-append>`. Add two attributes to `<dv-append>`, `text="{{ character.name }}"` and `append="is a filthy animal."` 

```
<h1>{{ team.name }}</h1>

<ul>
    <li ng-repeat="character in team.characters" dv-color="gold">
        <dv-append text="{{ character.name }}" append="is a filthy animal.">Jabberwocky</dv-append>
    </li>
</ul>

<hr>
<a ui-sref="main">Back To Main</a>
```

- Open `app/scripts/directives/dv-append.js` and add two variables like so: `template: '<div>{{ awesomeText }} {{ appendedText }}</div>'`
- Keep `restrict: 'E'`. We’re going to use the element style directive this time instead of the attribute style that we used for dv-color.
- Edit the `link` function to set `scope.awesomeText` and `scope.appendedText` to the text and append attributes.

```
'use strict';

angular.module('tmntApp')
  .directive('dvAppend', function () {
    return {
      template: '<div>{{ awesomeText }} {{ appendedText }}</div>',
      restrict: 'E',
      link: function postLink(scope, element, attrs) {
        scope.awesomeText = attrs.text;
        scope.appendedText = attrs.append;

      }
    };
  });
```

- Gawk at all of the filthy, golden animals! 

### dv-table
- Run `yo angular:directive dv-table`. Yeoman will create a new directive for you named dv-table.
- Open `team.html` and add a div anywhere you like with `dv-table=”team”`, where `team` is the name of the scope variable that holds your team data. Remember when you set it in `team.js`??? Set dv-table-headers as well. Make it an array with two items in it.

```
'use strict';

angular.module('tmntApp')
  .controller('TeamCtrl', function ($scope, team) {
    $scope.team = team;
  });
```
Now you can access `$scope.team` from `team.html` like so:

```
<div dv-table="team" dv-table-headers="['id', 'name']"></div>
```
You are actually going to pass in the scope reference for `team` into the dv-table directive.
- Open `app/scripts/directives/dv-table.js` and set `restrict` to A and set the `template` to the following text block.

```
<table>
	<thead>
		<tr>
			<th ng-repeat="header in dvTableHeaders">{{ header }}</th>
		</tr>
	</thead>
	<tbody>
		<tr ng-repeat="row in dvTable">
			<td ng-repeat="value in row">{{ value }}</td>
		</tr>
	</tbody>
</table>
```
- Add a `scope` attribute to your directive. This will create an isolate scope, and we’re going to use two-way binding to add `dvTable` and `dvTableHeaders` to the isolate scope.

```
scope: {
        dvTable: '=',
        dvTableHeaders: '='
      }
```
- Remove the `link` function. You don’t even need it! The finished product looks like this:

```
'use strict';

angular.module('tmntApp')
  .directive('dvTable', function () {
    return {
      template: '<table><thead><tr><th ng-repeat="header in dvTableHeaders">{{ header }}</th></tr></thead><tbody><tr ng-repeat="row in dvTable"><td ng-repeat="value in row">{{ value }}</td></tr></tbody></table>',
      restrict: 'A',
      scope: {
        dvTable: '=',
        dvTableHeaders: '='
      }
    };
  });
```
- Check out that hott, automagical table action!

---
### dv-popup
- Run `yo angular:directive dv-popup`. Yeoman will create a new directive for you named dv-popup.
- Open `main.html` and add a the following markup.

```
<button class="open-popup">Click to Open Popup</button>

<hr>

<div dv-popup="button">
    Hello world! I am a popup that you chose to open!!!
</div>
```

- Open `app/scripts/directives/dv-popup.js`
- Change the restrict from an element (E) to an attribute (A) restriction
- Add a template that looks like this

```
<div class="popup" style="display: none" ng-transclude></div>
```

- Set `transclude: true`
- Create a link function like this

```
link: function postLink(scope, element, attrs) {
        var selector = attrs.dvPopup,
          targets = angular.element(document.body).children(selector);

        targets.on('click', function () {
          element.find('.popup').toggle();
        });

      }
``` 

- Now click the button and notice that you get an error.
- Open up your debugger (command-option-j) in Chrome and open up `dv-popup.js` using command-O and typing the filename to search for it.
- Place a debugger on the line that’s throwing the error.
- Click the button, and when the debugger stops on the bad line, open up your console and experiment with ways to resolve the error.
- This is a tricky error, so don’t get discouraged. Think about what the line of code is trying to accomplish. Think about what’s going wrong. Try to find a way around it. Google ways to find elements in Angular. Google how to show and hide things in Angular.
- It might help to add jQuery to your application. You'll notice that you don't have access to all of jQuery's functionality... you're limited to Angular's [jQuery-lite functions](http://docs.angularjs.org/api/angular.element). Use ```bower install --save jquery``` and ```grunt bower-install``` to add jquery.js to your index.html file automagically. Angular will now wrap element in the full jQuery wrapper instead of the pared down jQuery-lite wrapper. This gives you nice functions like .hover and .children.
- Once you've debugged the errors in this directive, try showing and hiding the popup based on [jQuery's .hover function](http://api.jquery.com/hover/).
