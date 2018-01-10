# The Javascript Module Pattern
## Main Features 
* Reduces complications introduced through scoping, simplifies program design. 
* Easy to read
* Uses javascript objects by taking advantage of ‘this’ and js object prototypes. 
* Forces scope to only exist within the function or object. 
* Uses Immediately-Invoked-Function-Expressions to limit scope 
* 		    (function () {
			    //Code goes here  
		    })(); 
	* Declares a function which calls itself immediately 

## Modules 


### Creating Modules 
	let Module = (function () {
		//function initialization goes here
	})(); 

* Module is created in the global scope
* Only functions that are explicitly returned go back to the global scope. Otherwise they stay in the scope of the function and cannot be called outside of it.

### Immediately-Invoked Function-Expressions (IIEF)
    (function () {
	    //Code goes here  
    })(); 

* Creates a new scope for the function so run away scoped variables can’t be accidentally called. 
* Returns only the parts you need and leaves code out of the global scope. 
* Enables you to use more common variable names and consistent naming paradigms since they only exist in the scope of this function. 
* This technique can be used to emulate function or object privacy. 

#### Declaring private method(s) inside module 
	let Module = (function () {
		let privateMethod = function () {
			//Code goes here 
		}
	})(); 

#### Declaring public method(s) inside the module

	let Module = (function () {
		return {
			publicMethod: function () {
				// code goes here
			}, 
			anotherPublicMethod: function () {
				// code goes here
			}
		}
	})(); 

* returns public methods to Module as object literals, allowing us to call the functions as we would in any other object. 
	*  Module.publicMethod(); 
* If we made a normal object literal, we would be unable to make any private methods. 
	* Using an immediately invoked function allows our Module to have both public and private methods. 
	* The IIEF lets us throw logic into what we want and don't want to be available from the Module. 

#### Declaring both public and private method(s) inside the module

	let Module = (function () {
		let privateMethod = function () {}; 
		return {
			publicMethod: function () {
				// code goes here
			}, 
			anotherPublicMethod: function () {
				// code goes here
			}
		}
	})(); 

#### Declaring a locally scoped object literal 

	let Module = (function () {
		let myObject = {}; 
		let privateMethod() {};
		 
		myObject.publicMethod = function () {
			// take it away, Mr. Public Method! 
		}; 
		return myObject; 
	})(); 

#### Using private methods in conjunction to public methods in a module 

	let Module = (function () {
		let privateMethod = function (message) {
			console.log(`Hello, here is my message: ${message}`); 
		}; 
		let publicMethod = function (text) {
			privateMethod(text); 
		}
		return {
			publicMethod: publicMethod
		}
	})(); 
	Module.publicMethod('Hi there!'); 


#### Function hoisting in IIEF's 
* Normal declarations of functions, like so: function something () {}; 
	* This always hoists the functions to the top of the program, making it so they can be called from anywhere in the codebase. While convenient for general use, issues arise when debugging because it makes for extremely confusing and messy code. 
* function declaration  in this form: let myFunction = function () {}; 
	* Does not hoist the function, and as a result all functions must be declared before they are called and make for much cleaner and more maintainable code. 
	* Similar hoisting functionality to function declarations in java in c++. 

#### Private variable functionality in IIEF's 
* Private variables can be created just like private functions. As long as they aren't returned in the object, it will be unreachable from outside of the IIEF. 

#### Making child modules 
Let's say you want to have many modules which all extend the base functionality of our original module, here: 

	let ModuleOne = (function () {
		let privateMethod = function (message) {
			console.log(`Hello, here is my message: ${message}`); 
		}; 
		let publicMethod = function (text) {
			privateMethod(text); 
		}
		return {
			publicMethod: publicMethod
		}
	})(); 

You can make a child of this module by passing in the one above as a parameter. Once you've done that, simply add on to it, then return your edited module!  

	let ModuleTwo = (function (someModule) {
		Module.extension = function () {
			console.log("extension"); 
		}
		return Module; 
	})(ModuleOne || {}); 

	ModuleTwo.extension; 
	ModuleTwo.publicMethod("Here is my method!"); 

Since it's possible "ModuleOne" may be undefined, you can toss in a conditional || so if it is it'll simply use the empty object instead. 

### Private Naming Conventions

To distinguish between private and public functions or variables, it's best practice to pre-pend your private variables or functions with an underscore '_' .

Example: 

	let ModuleOne = (function () {
		let _privateMethod = function (message) {
			console.log(`Hello, here is my message: ${message}`); 
		}; 
		let _privateValue = 5; 
		let _privateArray = []; 
		let publicMethod = function (text) {
			privateMethod(text); 
		}
		return {
			publicMethod: publicMethod
		}
	})(); 
 
## How to use this thing
You've actually used this technique before, but without realizing it. 

 $(document).ready is essentially shorthand for this nested function: 
	
	(function($){
	   $(function(){
	      // Run on DOM ready
	   });
	})(jQuery);

Let's take a look at how we might use this in a project.

A normal project might look like this. 

	$(document).ready(function() {
	  var dummyVal = 5; 
	  var messageList = []; 
	  var customThingForThisPage= 'abracadabra';
	  
	  displaySpecialMessage(customThingForThisPage); 
	  	  
	  function init() {
	    // do a lot of things
	  }
	  
	  function fixesWeirdProblemWithThisSpecificPage() {
	    // code in here
	  }
	  
	  // all these functions get hoisted, meaning they can be called anywhere. 
	  function showMessage() {
	    // code in here
	  }
	  
	  function getMessage() {
	    // code in here
	  }
	  
	  function fixesProblemCausedByOtherFileInPage() {
	    // code in here
	  }
	  
	  function sendMessage() {
	    // code in here
	  }
	  
	  function displaySpecialMessage(message) {
	    // code in here
	  }
	});


## Why even do this? 
As we know, programming in sharepoint can turn into a nightmare if we let it. Be it from multiple js files getting pulled into the same page in multiple different content editors, or just tons of different code getting thrown in together. 

This module pattern lets us modulerize our code so we can not only easily share it with each other, but also know that when we pull different projects together nothing will conflict. 

* You'll be able to make reusable modules that can be used in any codebase, since they're only scoped to be within themselves, shut off from everything outside of them. 
* You'll be able to write clean, maintainable code that's easy to read and change. 
* You'll be able to debug much quicker, since you only need to look at the code partitioned within the module. 


### Example: 
Many projects being pulled into the same webpage will often throw errors due to multiple initializations of jQuery. Your project won't know which version to use, nor will anyone else's. And as a result, reduce the productivity of every dev working on the project. 

## Conclusion
Sharepoint has reserved words and characters, and might add more in the future. To make our code not only more maintainable for ourselves through readability, the module pattern greatly reduces ambiguity and scopes your code to only where it's being used.  

Sources: 
* [toddmotto - Mastering the module pattern](https://toddmotto.com/mastering-the-module-pattern/)
* [How should I initialize Jquery](https://stackoverflow.com/questions/2215904/how-should-i-initialize-jquery) 
* [What does (function($) {})(jQuery); mean?](https://stackoverflow.com/questions/2937227/what-does-function-jquery-mean)
