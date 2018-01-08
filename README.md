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

### Immediately-Invoked Function-Expressions (IIEF)
		    (function () {
			    //Code goes here  
		    })(); 

* Creates a new scope for the function so run away scoped variables can’t be accidentally called. 
* Returns only the parts you need and leaves code out of the global scope. 
* Enables you to use more common variable names and consistent naming paradigms since they only exist in the scope of this function. 
* This technique can be used to emulate function or object privacy. 

### Creating Modules 
	let Module = (function () {
		//function initialization goes here
	})(); 

* Module is created in the global scope
* Only functions that are explicitly returned go back to the global scope. Otherwise they stay in the scope of the function and cannot be called outside of it.
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
 


Sources: 
* [toddmotto - Mastering the module pattern](https://toddmotto.com/mastering-the-module-pattern/)
