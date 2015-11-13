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

    Only one AngularJS application can be auto-bootstrapped per HTML document. The first ngApp found in the document will be used to define the root element to auto-bootstrap as an application. To run multiple applications in an HTML document you must manually bootstrap them using angular.bootstrap instead. AngularJS applications cannot be nested within each other
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

1. Closure in loop

    ```
    function showHelp(help) {
        document.getElementById('help').innerHTML = help;
    }

    function setupHelp() {
        var helpText = [
            {'id': 'email', 'help': 'Your e-mail address'},
            {'id': 'name', 'help': 'Your full name'},
            {'id': 'age', 'help': 'Your age (you must be over 16)'}
        ];

        for (var i = 0; i < helpText.length; i++) {
            var item = helpText[i];
            document.getElementById(item.id).onfocus = function() {
                showHelp(item.help);
            }
        }
    }
    setupHelp();
    ```
    Fix 1

    ```
    for (var i = 0; i < helpText.length; i++) {
	var item = helpText[i];
	document.getElementById(item.id).onfocus = function() {
	    var help = item.help;
            return function(){
            	showHelp(help);
            }
        }();
    }
    ```

    Fix 2

    ```
    function makeHelp(help){return function(){showHelp(help);};}
    for (var i = 0; i < helpText.length; i++) {
        document.getElementById(item.id).onfocus = makeHelp(helpText[i].help);
    }
    ```

    Fix 3

    ```
    for (var i = 0; i < helpText.length; i++) {
        document.getElementById(item.id).onfocus= function(a) {
            return function() {
                showHelp(a);
            }
        }(helpText[i].help);
    }
    ```
    
1. ProtoType

    ```
    function Object1(){
        this.firstName="fname";
        this.lastName="lname";
        this.gender="gender";
        this.age="age";
        this.getLastName=function(){return this.lastName;};
    }

    Object1.prototype={
        getAge: function(){
            return this.age;
        },
        getGender: function(){
            return this.gender;
        }
    };

    Object1.prototype.getFirstName=function(){
        return this.firstName;
    };

    var obj = new Object1();
    console.log(obj.getFirstName());
    console.log(obj.getLastName());
    console.log(obj.getAge());
    
    ```
1. Create an object

    ```
    var s={};
    s.name="d1";
    s.age=31;
    s.show=function(){
	    alert(s.name+":"+s["age"]);
    };
    s.show();

    var s={
        name:"d2",
        age:32,
        show:function(){
            alert(s.name+":"+s["age"]);
        }
    };
    s.show();

    var s= new Object();
    s.name="d3";
    s.age=33;
    s.show=function(){
        alert(s.name+":"+s["age"]);
    };
    s.show();
    
    function Person(name,age){
        this.name=name;
        this.age=age;
        this.show=function(){
            alert(this.name+":"+this["age"]);
        };
    };
    var s = new Person("d4",34);
    s.show();
    ```
1. Function in object
    Two objects created by same class, their member functions will be different objects.
    ```
    function Person(name,age){
        this.name=name;
        this.age=age;
        this.show=function(){
            alert(this.name+":"+this["age"]);
        };
    };
    var s1 = new Person("d4",34);
    var s2 = new Person("d4",34);
    console.log(s1.constructor===Person);     //true
    console.log(s2.constructor===Person);     //true
    console.log(s1.show===s2.show);           //false
    ```
    If let them to share member function object, should make member function by using prototype. 
    ```
    function Person(name,age){
        this.name=name;
        this.age=age;
    };
    Person.prototype.show=function(){
        console.log(this.name+":"+this["age"]);
    };
    var s1 = new Person("d5",35);
    var s2 = new Person("d6",36);
    console.log(s1.constructor===Person);
    console.log(s2.constructor===Person);
    console.log(s1.show===s2.show);           //true
    
    Person.prototype.gender="male";
    console.log(Person.prototype.isPrototypeOf(s1));  //true
    console.log(s1.hasOwnProperty("name"));           //true
    console.log(s1.hasOwnProperty("gender"));         //true
    ```
    
1. Inheritance by call or apply
    ```
    function Animal(){
        this.s = "Animal";
        this.getName=function(){return this.getA() + this.getB();};
        this.getA=function(){};    //Likes a abstrct method;
    }
    function Cat(){
        Animal.call(this);
        this.name="Cat";
        this.getA=function(){return this.name;};    //Override
        this.getB=function(){return this.name.toUpperCase();};
    }
    function OtherCat(){
        Cat.call(this);
        //Unfortunately, the overloading likes in Java/C++/C# is not supported in js
        this.getC=function(c){return c+c;};   
        this.getC=function(c,d){return c+d;};
    }
    var cat = new Cat();
    console.log(cat.s);           //Animal
    console.log(cat.getName());   //CatCAT

    var caty = new OtherCat();
    console.log(caty.s);          //Animal
    console.log(caty.getName());  //CatCAT
    console.log(caty.getC("abc"));        //abcundefined, actually called getC("abc",undefined);
    console.log(caty.getC("abc","efg"));  //abcefg
    ```
1. Inheritance by prototype
    ```
    function Animal(){};
    Animal.prototype.s = "Animal";
    Animal.prototype.getName=function(){return this.getA() + this.getB();};
    Animal.prototype.getA=function(){return this.s;}; 
    function Dog(){
        this.name="dog";
        this.getA=function(){return this.name + "A";};    //Override
        this.getB=function(){return this.name + "B";};    //Delay declare
    };
    Dog.prototype=Animal.prototype;
    var dog = new Dog();
    console.log(dog.s);           //Animal
    console.log(dog.getName());   //dogAdogB  
    
    ```
1. Actually, above two ways are incorrect inheritance.         
    The previous one, child object cannot inherit the functions which are defined in prototype of parent. 
    See below code:

    ```
    function Animal(){
        this.s = "Animal";
    }
    Animal.prototype.getName = function(){return this.s};
    function Cat(){
        Animal.call(this);
    }
    var cat = new Cat();
    console.log(cat.s);           //Animal
    console.log(cat.getName());   //error "TypeError: cat.getName is not a function"
    
    ```
    The second one, child object cannot access the properties which are defined in the body of parent. 
    See below code:
    
    ```
    function Animal(){
        this.s = "Animal";
    };
    Animal.prototype.getName=function(){return "s:"+this.s;};

    function Dog(){
    
    };

    Dog.prototype=Animal.prototype;

    var ani = new Animal();
    console.log(ani.s);           //Animal  
    console.log(ani.getName());   //s:Animal 

    var dog = new Dog();
    console.log(dog.s);           //undefined 
    console.log(dog.getName());   //s:undefined 

    ```
    
    Of course, we can fix the problem in the second one, add call function to Dog function as follows:
    
    ```
    ...
    function Dog(){
        Animal.call(this);
    };
    Dog.prototype=Animal.prototype;

    var ani = new Animal();
    console.log(ani.s);           //Animal  
    console.log(ani.getName());   //s:Animal 

    var dog = new Dog();
    console.log(dog.s);           //Animal 
    console.log(dog.getName());   //s:Animal 
    
    ```
    
    Seems everything is OK? No, there is a fatal problem in above. When you add a function to child by using prototype, at the same time, this function also will be added in to parent. Because their prototypes are one and the same. See the below code:
    
    ```
    
    function Animal(){
        this.s = "Animal";
        this.gender="unknown";
    };
    Animal.prototype.getName=function(){return "s:"+this.s;};

    function Dog(){
        Animal.call(this);
        this.gender = "Female";
    };

    Dog.prototype=Animal.prototype;
    Dog.prototype.getGender=function(){return this.gender;};

    var ani = new Animal();
    console.log(ani.s);           //Animal  
    console.log(ani.getName());   //s:Animal 
    console.log(ani.getGender()); //unknown ----- Nobody whats it!

    var dog = new Dog();
    console.log(dog.s);           //Animal 
    console.log(dog.getName());   //s:Animal  
    console.log(dog.getGender()); //'Female'

    ```
    To fix above problem as below, should be OK:
    
    ```
    Dog.prototype=Object.create(Animal.prototype);
    ```
    
    But, But, meanwhile Dog.prototype.constructor has been set to Animal, we have to change it to Dog.
    
    ```
    function Animal(){
        this.s = "Animal";
    };
    Animal.prototype.getName=function(){return "s:"+this.s;};

    function Dog(){
        Animal.call(this);
        this.s="Dog";  
    };

    Dog.prototype=Object.create(Animal.prototype);
    console.log(Dog.prototype.hasOwnProperty('constructor')); // false
    console.log(Dog.prototype.constructor===Animal);//true
    Dog.prototype.constructor=Dog;
    console.log(Dog.prototype.hasOwnProperty('constructor')); // false
    console.log(Dog.prototype.constructor===Dog);//true

    var dog = new Dog();
    console.log(dog.getName());
    ```
    
    All of these problems cause the mechanis of prototype and \_\_proto\_\_. Basiclly, under the relationship of parent and child, should be guaranteed that child.prototype.\_\_proto\_\_===parent.prototype. 
    
    About constructor:
    The constructor property makes absolutely no practical difference to anything internally. It's only any use if your code explicitly uses it. For example, you may decide you need each of your objects to have a reference to the actual constructor function that created it; if so, you'll need to set the constructor property explicitly when you set up inheritance by assigning an object to a constructor function's prototype property, as in the example.
    
1. The Singleton Pattern
    The Singleton pattern is thus known because it restricts instantiation of a class to a single object. Classically, the Singleton pattern can be implemented by creating a class with a method that creates a new instance of the class if one doesn't exist. In the event of an instance already existing, it simply returns a reference to that object.

Singletons differ from static classes (or objects) as we can delay their initialization, generally because they require some information that may not be available during initialization time. They don't provide a way for code that is unaware of a previous reference to them to easily retrieve them. This is because it is neither the object or "class" that's returned by a Singleton, it's a structure. Think of how closured variables aren't actually closures - the function scope that provides the closure is the closure.

In JavaScript, Singletons serve as a shared resource namespace which isolate implementation code from the global namespace so as to provide a single point of access for functions.

We can implement a Singleton as follows:

    ```
    var mySingleton = (function () {
        // Instance stores a reference to the Singleton
        var instance;
        function init() {
            // Singleton
            // Private methods and variables
            function privateMethod(){
                console.log( "I am private" );
            }
 
            var privateVariable = "Im also private";
            var privateRandomNumber = Math.random();
            return {
                // Public methods and variables
                publicMethod: function () {
                    privateMethod();
                    console.log( "The public can see me!" );
                },
                publicProperty: "I am also public",
                getRandomNumber: function() {
                    return privateRandomNumber;
                }
            };
        };
        return {
            // Get the Singleton instance if one exists
            // or create one if it doesn't
            getInstance: function () {
                if ( !instance ) {
                    instance = init();
                }
                return instance;
            }
        };
    })();
    
    var singleA = mySingleton.getInstance();
    console.log(singleA.getRandomNumber()); // some number;
    console.log(singleA.publicProperty);    // I am also public
    singleA.publicMethod();                 // I am private, The public can see me!
    singleA.privateMethod();                // Error
    
    ```
