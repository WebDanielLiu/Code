1. You have the following JavaScript code. Write the appropriate HTML so all three statements are equivalent (will bring the same value).

		javascript:
		document.forms["myForm"].elements["myField"].value
		document.forms[1].elements[1].value
		document.myForm.myField.value
		
	1.1. Answer:
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

1. Read the API on object String and Array.
	[http://docs.sencha.com/extjs/4.2.1/#!/api/String](http://docs.sencha.com/extjs/4.2.1/#!/api/String)
	[http://docs.sencha.com/extjs/4.2.1/#!/api/Array](http://docs.sencha.com/extjs/4.2.1/#!/api/Array)

	Then write a one-line function (less than 80 characters) that returns a boolean indicating whether or not a string is a palindrome. e.g. "A nut for a jar of tuna." is a palindrome.
	
	1.1. Answer:
	(function(a){var b=a.replace(/[.\s]/g,'').toUpperCase().split('');return b.join('')==b.reverse().join('');})("A nut for a jar of tuna.");
	
	
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

	1.1. Answer:
		outer func: this.foo = bar;
		outer func: this.foo = bar;
		inner func: this.foo = undefined;
		inner func: this.foo = bar;
	
		outer func: this.foo = undefined;
		outer func: this.foo = undefined;
		inner func: this.foo = undefined;
		inner func: this.foo = undefined;

1. Implement the following function. It should return the sum of the two numbers:

		sum(3)(4); // returns 7

1. Tell me what the function below does. Do you see the bug?

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
	1.1. Answer:
		It is to find the second maximum number in an array.
		If the array has duplicate values, the function cannot work correctly.
		It should be fixed as below:
		if (num > Math.min(a1, a2) && num != a1 && num != a2){
		
1. Highlights in resume
	?
1. Explain what is MVC
	1.1. Answer:		
		MVC stands for Model-View-Controller. It's a design pattern that breaks an application into three parts:
		* the data(Model), the presentation of that data to the user(View), and the actions taken on any user interaction(Controller).
		* Model, which stores an application data model; View, which renders Model for an appropriate respresentation;
		and Controller, which updates Model.
		
		In other words, a user does something. That "something" is passed to a Controller that controls what should happen next.
		Offten, the Controller requests data from the Model and then gives it to the View, which presents it to the user. 
		
		But what does this separation mean for a website or web application?
		
			
1. Do the quiz. [http://www.w3schools.com/quiztest/quiztest.asp?qtest=JavaScript](http://www.w3schools.com/quiztest/quiztest.asp?qtest=JavaScript)
	1.1. Answer:
		Done.
1. watch video [https://www.youtube.com/watch?v=W9goxQO6CrY](https://www.youtube.com/watch?v=W9goxQO6CrY)
	1.1. Answer:
		This video is no longer available because the YouTube account associated with this video has been terminated.
1. Implement the screenshot below using ExtJS
	* [screen1.png](img/screen1.png)
	* [screen2.png](img/screen2.png)

