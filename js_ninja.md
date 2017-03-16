# Resig
## Debugging
- Firebug, Chrome Dev Tools, IE Dev Tools, Opera Dragonfly, Safari WebKit 
### Logging
- Not uniform across browsers

        function log() {
          try {
            console.log.apply(console, arguments);
          } catch(e) {
            try {
              opera.postError.apply(opera, arguments);
            } catch(e){
              alert(Array.prototype.join.call( arguments, " "));
            }
          } 
        }

### Breakpoints
- Set in debugger in margin of code line
- Set using debugger; inside code, can also create functions that do it programmatically
- Step into - descend into method calls on breakpointed line, executing code one statement at a time
- Step over - proceeds to the next line without descending into any method calls on the current line, similar to step into but executes each set of statements as a single unit
- Step out - executes up to the next return (i.e. the ending of the function call with control of the current scope when the debugger is invoked) and gives control to the containing scope
- Use the pause on exception button to stop execution any time an exception is thrown and inspect the stack, can pause on all, stop on uncaught exceptions or ignore
### Testing
- Repeatability - same results on multiple runs, Simplicity - each test should test just one thing, Independence - each test should execute in isolation
- Deconstructive or constructive - whittle down to problem or start from a base well functioning position and build up
- Jasmine, QUnit, Mocha, YUI Test, Cucumber JS, jsunit
- Unit test: Specify and test one point of the contract of single method of a class. This should have a very narrow and well defined scope. Complex dependencies and interactions to the outside world are stubbed or mocked.
- Test types: 
  - Integration test: Test the correct interoperation of multiple subsystems. Test a whole spectrum of features in a logical larger unit, i.e. testing correct operation between two classes or testing the overall integration with the production environment.
  - Regression test: A test that is written when a bug is fixed. It ensures that the specific bug will not occur again. The full name is "non-regression test". It can also be a test made prior to changing an application to make sure the application provides the same outcome.
  - Smoke test (aka Sanity check): A simple integration test where we just check that when the system under test is invoked it returns normally and does not blow up. It is an analogy with electronics, where the first test occurs when powering up a circuit: if it smokes, it's bad.
  - Acceptance test: Test that a feature or use case is correctly implemented. It is similar to an integration test, but with a focus on the use case rather than on the components involved.
  - System test: Test a system as a black box. Dependencies on other systems are often mocked or stubbed during the test (otherwise it would be more of an integration test).
  - Pre-flight check: Tests that are repeated in a production-like environment, to alleviate the 'builds on my machine' syndrome. Often this is realized by doing an acceptance or smoke test in a production like environment
### Fundamentals
- Assert - most basically asserts that a value is a value, more importantly checks that a function returns a value (i.e. true when true is expected, etc) along with what to do with the result
- Basic JS

        function assert(value, desc) {
          var li = document.createElement("li");
          li.className = value ? "pass" : "fail";
          li.appendChild(document.createTextNode(desc));
          document.getElementById("results").appendChild(li);
        }
        window.onload = function() {
          assert(true, "The test suite is running.");
          assert(false, "Fail!");
        };

- Test groups - a way to run and display logically grouped test results together. Group interconnected tasks together and their tests together.
- Asynchronous testing - Must be able to handle processes that will return after an unknown time
  - Group test logically based on the same async operations
  - Queue each async test group to be run after all previous test groups have finished
  
          (function() {
            var queue = [], paused = false, results;
            this.test = function(name, fn) {
              queue.push(function() {
                results = document.getElementById("results");
                results = assert(true, name).appendChild(
                document.createElement("ul"));
                fn(); 
              });
              runTest();
            };
            this.pause = function() {
              paused = true;
            };
            this.resume = function() {
              paused = false;
              setTimeout(runTest, 1);
            };
            function runTest() {
              if (!paused && queue.length) {
                queue.shift()();
                if (!paused) {
                  resume(); 
                }
              } 
            }
            this.assert = function assert(value, desc) {
              var li = document.createElement("li");
              li.className = value ? "pass" : "fail";
              li.appendChild(document.createTextNode(desc));
              results.appendChild(li);
              if (!value) {
                li.parentNode.parentNode.className = "fail";
              }
              return li; 
            };
          })();
          window.onload = function() {
            test("Async Test # 1", function() {
              pause();
              setTimeout(function() {
                assert(true, "First test completed");
                resume();
              }, 1000);
            });
            test("Async Test # 2", function() {
              pause();
              setTimeout(function() {
                assert(true, "Second test completed");
                resume();
              }, 1000);
            }); 
          };

- Takes a function containing multiple assertions that are grouped sync or async and places the function on the queue
- Uses pause and resume to start and stop tests, runTest to check the queue for running test

## 3 - Functions
- Primary modular unit of execution
- First-class objects 
  - Created via literals
  - assigned to vars, arrays, and properties of other objects
  - passed as arguments to other functions
  - returned as values from functions
  - possess properties that are dynamically created and assigned
- The event loop - set up the interface, wait for events, invoke handlers or listeners to respond to events, return to loop
- The event loop is FIFO
- A function set up to be called at a later time is a callback
- Array.sort called with no callback simply sorts ascending

        values.sort(function(value1,value2){ return value2 - value1; });

    This signals to sort to reverse numbers if value is negative
### Declarations

      function name(params) {return; }

- Basic testing:
  - Assert typeof === "function"
  - Assert name of function (func.name, "" for unnamed)
  - Assert the object that the function is a property of 
### Scoping
- scoping is contained by functions (block scoping in ES6) i.e. lexical scoping
- Subtle nuances - hoisting of variables and functions (even though Resig says vars are in scope from point of declaration to end of function), named functions in scope within an entire function, global context is one big function scope
- Testing scoping

        assert(true,"|----- BEFORE OUTER -----|");
        /* test code here */

        ￼function outer(){
          assert(true,"|----- INSIDE OUTER, BEFORE a -----|");
          /* test code here */
  
          ￼var a = 1;
  
          assert(true,"|----- INSIDE OUTER, AFTER a -----|");
          /* test code here */
  
          function inner(){ /* does nothing */ }
          var b = 2;

          assert(true,"|----- INSIDE OUTER, AFTER inner() AND b -----|"); /* test code here */

          if (a == 1) {
            var c = 3;
            assert(true,"|----- INSIDE OUTER, INSIDE if -----|");
            /* test code here */
          }
  
          assert(true,"|----- INSIDE OUTER, OUTSIDE if -----|");
          /* test code here */
        } 

        outer();

        assert(true,"|----- AFTER OUTER -----|");
        /* test code here */

### Invocations
- as a function, as a method, as a constructor with new, using call or apply
- () is the invocation operator for functions, methods, and and constructors
- arguments passed to a function on invocation are passed as the parameters in the original declarations in the order
- handling of more or less arguments than parameters
  - more - extra args are simply not assigned
  - less - params missing args are set to undefined within invocation
- All function invocations receive this and arguments
- arguments is a collections (not array)
- this - global object (window) for top level declared functions (function within a function receives the this of the containing function? actually receives global scope for function declaration within a method), the object that contains the method for method calls, explicitly assigned this object for call and apply, or the object being created with new or Object.create 
- Things to consider with new - do not use a constructor without new because all properties are attached to the global object
- Apply and call 

        function juggle() {
          var result = 0;
          for (var n = 0; n < arguments.length; n++) {
            result += arguments[n];
          }
          this.result = result;
        }
  
        var ninja = {};
        var ninja2 = {};

        juggle.apply(ninja1, [1, 2, 3, 4]);

        juggle.call(ninja2, 5, 6, 7, 8);

        assert(ninja1.result === 10, "juggled via apply");
        assert(ninja2.result === 26, "juggled via call");

- Imperative programming passes a collection to a function and iterates through the array, calling the operation on each element. The functional programming approach is to create a function and pass each element of the collection to that function
- Simplified forEach

        function forEach(list, callback) {
          for (var n = 0; n < list.length; n++) {
            callback.call(list[n], n);
          }
        }

## 4 - Function Use
### Anonymous functions
 - used with variable assignment or method assignment (name is name of property, not of function
### Recursion
- More common with named functions 
- Palindrome test

        function isPalindrome(text) {
          if (text.length <= 1) return true;
          if (text.charAt(0) != text.charAt(text.length - 1)) return false;
          return isPalindrome(text.substr(1, text.length - 2));
        }

- Recursion criteria - reference to self and convergence towards termination
- Recursion with methods - just like named function but call with object.method
- When using prototypal inheritance with a method and recursion, any reassignment of the parent object changes the descendent (pilfered reference)
- Can fix in the initial function by referencing the object using this rather than the object itself
### Inline Named Functions
- Previous example relies on all methods using same name when inheriting. Recursion in this situation can be resolved by using an inline function (naming an anonymous function)
- Can also use inline functions for variable assignments and debugging
- Inline function names are only visible within the functions themselves
- Can also use the arguments.callee property (though it is not good practice and is removed from ES5). This is used to access the currently executing function (ie for recursion)
### Functions as objects
- Have all features of objects, but are callable
- Storing functions in collections - for unique callbacks and more. Create an object with a nextId, a cache object for storing functions, and an add method which checks for a functions id, sets it to the next function, and then adds it to the cache
- Use !! before an expression to typecast the expression to its boolean equivalent
- Can use function properties for functions that modify themselves (self-memoization)
- Memoization reduces performance cost by cutting down on need for unnecessary computation on consecutive calls
  - avoid function calls for previously computed answers
  - seamlessly happens behind the scenes
  - sacrifices memory in favor of performance
  - some view caching as a concern that should not be mixed with business logic
  - more difficult to test performance of an algorithm like this
- Good to use caching for querying for DOM elements by tag name (5x performance increase)
- To create array-like objects that contain metadata can create an object and array methods to the object by using call and this
### Variable Arity
- Use arguments and apply, supplying multiple arguments to functions that can accept any number of them
- Variable length arg lists for function overloading
- Using length property of the arguments list
- objectMethod.apply(thisArg, [argsArray]) - use for variable arity functions
