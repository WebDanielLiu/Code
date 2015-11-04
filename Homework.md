1. You have the following JavaScript code. Write the appropriate HTML so all three statements are equivalent (will bring the same value).

		javascript:
		document.forms["myForm"].elements["myField"].value
		document.forms[1].elements[1].value
		document.myForm.myField.value
	* Answer:
	```
	<!DOCTYPE html>
	<html>
	<head></head>
	<body>
		<form name="myForm0" id="myForm">
			<input id="myField"/>
		</form>
		<form name="myForm1">
		    <input id="myField"/>
			<input id="myField1"/>
		</form>
		<form name="myForm">
		    <input id="myField"/>
		</form>

		<script>
    		document.forms["myForm"].elements["myField"].value=1;	// Access by id
    		document.forms[1].elements[1].value=2;					// Access by tag && array
    		document.myForm.myField.value=3;						// Access by name
		</script>
	</body>
	</html>
	```
1. Read the API on object String and Array.
	[http://docs.sencha.com/extjs/4.2.1/#!/api/String](http://docs.sencha.com/extjs/4.2.1/#!/api/String)
	[http://docs.sencha.com/extjs/4.2.1/#!/api/Array](http://docs.sencha.com/extjs/4.2.1/#!/api/Array)

	Then write a one-line function (less than 80 characters) that returns a boolean indicating whether or not a string is a palindrome. e.g. "A nut for a jar of tuna." is a palindrome.
	
	* Answer:
	```
	(function(a){var b=a.replace(/[.\s]/g,'').toUpperCase().split('');return b.join('')==b.reverse().join('');})("A nut for a jar of tuna.");
	```
	
	
1. Read this article then run the following code in a browser and explain the result. 

	[http://ryanmorr.com/understanding-scope-and-context-in-javascript/](http://ryanmorr.com/understanding-scope-and-context-in-javascript/)


		var myObject = {
			foo: "bar",
			func: function() {
				var self = this;
				console.log("outer func:  this.foo = " + this.foo);
				console.log("outer func:  self.foo = " + self.foo);
				(function() {
					console.log("inner func:  this.foo = " + this.foo);
					console.log("inner func:  self.foo = " + self.foo);
				})();
			}
		};
		myObject.func();
		var test = myObject.func;
		test();

	* Answer:
	```
	outer func: this.foo = bar;
	outer func: this.foo = bar;
	inner func: this.foo = undefined;
	inner func: this.foo = bar;
	
	outer func: this.foo = undefined;
	outer func: this.foo = undefined;
	inner func: this.foo = undefined;
	inner func: this.foo = undefined;
	```

1. Implement the following function. It should return the sum of the two numbers:

	sum(3)(4); // returns 7
	* Answer:
	```
	function sum(x){
		return function(y){
			return x + y;
		};
	}
	```

1. Tell me what the function below does. Do you see the bug?
	```
	function foo(arr) {
		var a1 = -Infinity, a2 = -Infinity;
		arr.forEach(function(num) {
			if (num > Math.min(a1, a2)) {
				if (a1 < a2) {
					a1 = num;
				} else {
					a2 = num;
				}
			}
		});
		return (Math.min(a1, a2));
	}
	```
	* Answer:
	
	It is to find the second maximum number in an array.
	If the array has duplicate values, the function cannot work correctly.
	It should be fixed as below:
	```
	if (num > Math.min(a1, a2) && num != a1 && num != a2){
	```
		
1. Highlights in resume
	?
1. Explain what is MVC
	* Answer:		
	MVC stands for Model-View-Controller. It's a design pattern that breaks an application into three parts:
	I. the data(Model), the presentation of that data to the user(View), and the actions taken on any user interaction(Controller).
	II. Model, which stores an application data model; View, which renders Model for an appropriate respresentation;and Controller, which updates Model.
	In other words, a user does something. That "something" is passed to a Controller that controls what should happen next.
	Offten, the Controller requests data from the Model and then gives it to the View, which presents it to the user. 
	But what does this separation mean for a website or web application?
		
			
1. Do the quiz. [http://www.w3schools.com/quiztest/quiztest.asp?qtest=JavaScript](http://www.w3schools.com/quiztest/quiztest.asp?qtest=JavaScript)
	* Answer:
	Done.
1. watch video [https://www.youtube.com/watch?v=W9goxQO6CrY](https://www.youtube.com/watch?v=W9goxQO6CrY)
	* Answer:
	This video is no longer available because the YouTube account associated with this video has been terminated.
1. Implement the screenshot below using ExtJS
	* [screen1.png](img/screen1.png)
	* [screen2.png](img/screen2.png)
	
1. server-side rendering vs client-side rendering
1. implement function add(a, b, c...) 

	```
	add(3); // returns 3 
	add(3, 4); // returns 7 
	add(3, 4, 5); // returns 12

	function add() {
    	var i, a = 0;
    	for (i in arguments) {
        	a += arguments[i] ;
    	}
    	return a;
	}
	```
1. impplement function max(a, b, c...) 

	```
	max(3); // returns 3 
	max(3, 4); // returns 4 
	max(3, 4, 5); // returns 5
	function max() {
    	var i, m = -Infinity;
    	for (i in arguments) {
        	if (arguments[i] > m) {
            	m = arguments[i];
        	}
    	}
    	return m;
	}
	```
1. implement function clone
	```
	var x = function() {
    	return 1;
	};

	var t = function(a,b,c) {
    	return a+b+c;
	};

	Function.prototype.clone = function() {
    	var that = this;
    	var temp = function temporary() { return that.apply(this, arguments); };
    	for(var key in this) {
        	if (this.hasOwnProperty(key)) {
            	temp[key] = this[key];
        	}
    	}
    	return temp;
	};

	alert(x === x.clone());					//false
	alert(x() === x.clone()());				//true

	alert(t === t.clone());					//false
	alert(t(1,1,1) === t.clone()(1,1,1));	//true
	alert(t.clone()(1,1,1));				//3
	```
1. AngularJS Multiple ng-app within a page
```
<div ng-app="myApp" ng-controller="myCtrl">
{{ firstName + " " + lastName }}
</div>
<div id="myApp1" ng-app="myApp1" ng-controller="myCtrl1">
{{ firstName1 + " " + lastName1 }}
</div>
<div id="myApp2" ng-app="myApp2" ng-controller="myCtrl2">
{{ firstName2 + " " + lastName2 }}
</div>

<script>
var app = angular.module("myApp", []);
app.controller("myCtrl", function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
});
var app1 = angular.module("myApp1", []);
app1.controller("myCtrl1", function($scope) {
    $scope.firstName1 = "John";
    $scope.lastName1 = "Doe";
});
angular.bootstrap(document.getElementById("myApp1"),['myApp1']);

var app2 = angular.module("myApp2", []);
app2.controller("myCtrl2", function($scope) {
    $scope.firstName2 = "John";
    $scope.lastName2 = "Doe";
});
angular.bootstrap(document.getElementById("myApp2"),['myApp2']);
</script>
```
