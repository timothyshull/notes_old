## The Module Pattern
- Note: Addy Osmani's definition of an IIFE requires that it returns a function but it does not seem that others require this distinction
- two de facto module formats have evolved. Asynchronous Module Definition (AMD) is the most popular for client-side code, while node.js modules (an extension to CommonJS Modules/1.1) is the leading pattern in server-side environments. Universal Module Definition (UMD) is a set of boilerplate recipes that attempt to bridge the differences between AMD and node.js, allowing engineers to author their code bases in a single format, rather than author in both formats or convert to the other format in a build step.
- Both CommonJS and AMD require script loaders meaning they do not automatically function on their own. 
- Dependency management loaders - RequireJS, LoadRunner, Dojo, CurlJS
- Generic script loaders - LabJS, YepnopeJS, A.getJS
- Browserify is used for client-side dependencies and reads all of your scripts/modules and compiles them into one to be loaded in your page. It allows the inclusion of node modules for use in the browser. 
### Options
1. The Module Pattern
2. Object literal notation
3. AMD modules
4. CommonJS modules
5. ECMAScript Harmony modules
### The Module Pattern
1. The starting point is an IIFE

        (function () {
          // code
        })();

    This creates a new scope (lexical scoping in JavaScript with proposed block scoping in ES 6). 
2. To namespace the scope, we assign it to a variable

        var Module = (function () {
          // code
        })();

3. This pattern is designed to further emulate concepts in class based languages in the context of JavaScript's prototypal nature
#### Private methods
1. Using this pattern, it is possible to create private methods. They are used to hide access to users, developers, or hackers. Something like server calls should not be accessible to the general public. 
2. The other purpose is to avoid polluting the global namespace to avoid naming collisions. 
3. Only the public API is returned
3. Basic:

        var Module = (function () {
  
          var privateMethod = function () {
            // do something
          };

        })();

4. This makes any attempt to call privateMethod in the global environment throw an error
5. Note: JavaScript does not include a true notion of privacy but this pattern takes advantage of function scope for the benefits produced by privacy 
#### Namespaced access
1. The idea is to return an anonymous object containing the method which is then attached to the variable that the IIFE is being assigned to.

        var Module = (function () {
  
          return {
            publicMethod: function () {
              // code
            }
          };

        })();
        
        Module.publicMethod();

2. This is similar in use to basic object literal notation
#### Anonymous Object Literal Return

        var Module = (function () {

          var privateMethod = function () {};
  
          return {
            publicMethodOne: function () {
              // I can call `privateMethod()` you know...
            },
            publicMethodtwo: function () {

            },
            publicMethodThree: function () {

            }
          };

        })();

- Memoization potential
        
        var testModule = (function () {
 
          var counter = 0;
 
          return {
 
            incrementCounter: function () {
              return counter++;
            },
 
            resetCounter: function () {
              console.log( "counter value prior to reset: " + counter );
              counter = 0;
            }
          };
 
        })();

- Simple template

        var myNamespace = (function () {
 
          var myPrivateVar, myPrivateMethod;
 
          // A private counter variable
          myPrivateVar = 0;
 
          // A private function which logs any arguments
          myPrivateMethod = function( foo ) {
              console.log( foo );
          };
 
          return {
 
            // A public variable
            myPublicVar: "foo",
 
            // A public function utilizing privates
            myPublicFunction: function( bar ) {
 
              // Increment our private counter
              myPrivateVar++;
 
              // Call our private method using bar
              myPrivateMethod( bar );
 
            }
          };
 
        })();

#### Locally Scoped Object Literal
1. This is a common use in libraries. As the methods are defined, they are either private if not a attached to the object (empty object declared at the top of the IIFE and returned at the bottom) or public if they are attached.

        var Module = (function () {

          // locally scoped Object
          var myObject = {};

          // declared with `var`, must be "private"
          var privateMethod = function () {};

          myObject.someMethod = function () {
            // take it away Mr. Public Method
          };
  
          return myObject;

        })();

2. The name of the internal object is unimportant in this case. 
#### Stacked Locally Scoped Object Literal
1. Pretty much the same but slightly different syntax

        var Module = (function () {

          var privateMethod = function () {};

          var myObject = {
            someMethod:  function () {

            },
            anotherMethod:  function () {
      
            }
          };
  
          return myObject;

        })();

2. Be aware of hoisting and it's effects on the way you define your functions and objects.
#### Revealing Module Pattern
1. This is a variation where the returned object reveals public pointers to private methods. This is good for code management because you can clearly see which functions are returned. 

        var Module = (function () {

          var privateMethod = function () {
            // private
          };

          var someMethod = function () {
            // public
          };

          var anotherMethod = function () {
            // public
          };
  
          return {
            someMethod: someMethod,
            anotherMethod: anotherMethod
          };

        })();

2. The value of the revealing module pattern is in its visual clarity, the lack of a need for syntax switching (object literal notation) for public methods, and rewriting the initial object name when adding methods 

        var myRevealingModule = (function () {
 
                var privateVar = "Ben Cherry",
                    publicVar  = "Hey there!";
 
                function privateFunction() {
                    console.log( "Name:" + privateVar );
                }
 
                function publicSetName( strName ) {
                    privateVar = strName;
                }
 
                function publicGetName() {
                    privateFunction();
                }
 
 
                // Reveal public pointers to
                // private functions and properties
 
                return {
                    setName: publicSetName,
                    greeting: publicVar,
                    getName: publicGetName
                };
 
            })();
 
        myRevealingModule.setName( "Paul Kinlan" );

3. One disadvantage is that if a private function refers to a public function, the public function cannot be overridden
#### Accessing Private Methods
1. Private methods can be invoked via the public methods

        var Module = (function () {

          var privateMethod = function (message) {
            console.log(message);
          };

          var publicMethod = function (text) {
            privateMethod(text);
          };
  
          return {
            publicMethod: publicMethod
          };

        })();

        // Example of passing data into a private method
        // the private method will then `console.log()` 'Hello!'
        Module.publicMethod('Hello!');

2. You can also access any other type of object

        var Module = (function () {

          var privateArray = [];

          var publicMethod = function (somethingOfInterest) {
            privateArray.push(somethingOfInterest);
          };
  
          return {
            publicMethod: publicMethod
          };

        })();

3. To augment modules, see augmenting modules elsewhere
4. Naming conventions - for quick scanning, use _ to name the private methods

### Object Literal Notation
1. This is simply the normal use of objects to encapsulate data and methods 
2. Described as a set of comma separated name value pairs enclosed in curly braces (no comma after final value)
3. The module pattern's essential difference is that it returns an object from a scoping function (IIFE)

        var myObjectLiteral = {
 
            variableKey: variableValue,
 
            functionKey: function () {
              // ...
            }
        };

        var myObjLiteral = {
          defaults: { name: 'Todd' },
          someMethod: function () {
            console.log(this.defaults);
          }
        };

        // console.log: Object { name: 'Todd' }
        myObjLiteral.someMethod();

### Import Mixins
1. Global variables and functions can be imported into the module and locally alias them if necessary

        // Global module
        var myModule = (function ( jQ, _ ) {
 
            function privateMethod1(){
                jQ(".container").html("test");
            }
 
            function privateMethod2(){
              console.log( _.min([10, 5, 100, 2, 1000]) );
            }
 
            return{
                publicMethod: function(){
                    privateMethod1();
                }
            };
 
        // Pull in jQuery and Underscore
        })( jQuery, _ );
 
        myModule.publicMethod();

### Protecting Properties

        var store = window.store || {};
 
        if ( !store["basket"] ) {
          store.basket = {};
        }
 
        if ( !store.basket["core"] ) {
          store.basket.core = {};
        }
 
        store.basket.core = {
          // ...rest of our logic
        };

### jQuery Module Pattern 

        function library( module ) {
 
          $( function() {
            if ( module.init ) {
              module.init();
            }
          });
 
          return module;
        }
 
        var myLibrary = library(function () {
 
          return {
            init: function () {
              // module implementation
            }
          };
        }());

### Discussion
- Advantages - cleaner and simpler method for encapsulation, doesn't pollute the global namespace, supports private data and protects it from the outside world

## AMD Modules
1. Just a proposal for common approach to async loading of modules and dependencies. Used by Dojo, MooTools, Firebug, and jQuery. 
2. Advantages
    - Asynchronous by nature
    - Dependencies are easy to identify
    - Avoids global variables
    - Can be lazy loaded if needed
    - Easily portable
    - Can load more than JavaScript files
    - Powerful plugin support
    - Support script fallbacks (OMG, CDN is down!)
    - Can be easily configured
    - Can load multiple versions of the same library
### Basics
1. The module definition

        define(
            module_id /*optional*/, 
            [dependencies] /*optional*/, 
            definition function /*function for instantiating the module or object*/
        );

2. The module_id is generally only needed when non-AMD concatenation tools are being used, but is also equivalent to folder paths.
3. r.js is part of require.js, it is a script for running AMD-based projects in Node, Nashorn, Rhino and xpcshell and it includes a RequireJS optimizer for optimal browser delivery.'
4. An AMD environment provides a single global function, define. The define function has several signatures, including some that trigger special behavior, but the simplest form accepts an array of module ids and a factory function. An AMD module should export something by returning a value from the factory function.
5. Define comes from whichever library you use to implement this API (i.e. require js, 


### AMD Spec
1. Use (see amdjs on github, also generally intended for use with backbone and underscore). The define function is required by the specification to be available as a free variable or global function

        define(id?, dependencies?, factory);

2. id - module being defined, camel case, no extension, can include pathname directives "../", dependencies - array literal of the module ids that are required dependencies of the module being defined, factory - a function or object used to instantiate the module
3. Simplified CommonJS - if dependencies are omitted, the factory function can include require statements with module ids
4. Any global define function should have a property called amd to avoid conflict with other JavaScript code that includes a define function. The minimum definition

        define.amd = {};

    Definition example for loading more than one version of a module
    
            define.amd = {
                multiversion: true
            };

5. Sets up alpha, that uses beta, and require and exports

            define("alpha", ["require", "exports", "beta"], function (require, exports, beta) {
                exports.verb = function() {
                    return beta.verb();
                    //Or:
                    return require("beta").verb();
                }
            });

6. Simplified anonymous module

        define(["alpha"], function (alpha) {
            return {
                verb: function(){
                    return alpha.verb() + 2;
                }
            };
        });

7. Dependency free modules can just define an object literal 

        define({
            add: function(x, y){
                return x + y;
            }
        });

8. Simplified CommonJS

        define(function (require, exports, module) {
            var a = require('a'),
            b = require('b');

            exports.action = function () {};
        });

### Relation to CommonJS
1. AMD can be used as a transport format for CommonJS as long as the module does not use computed, synchronous require calls. Code like this can be converted to the async callback require used by AMD 
### Loader Plugins
1. Requirements load: function (resourceId, require, load, config), normalize: function (resourceId, normalize), dynamic: Boolean

        (function () {

            //Helper function to parse the 'N?value:value:value'
            //format used in the resource name.
            function parse(name) {
                var parts = name.split('?'),
                    index = parseInt(parts[0], 10),
                    choices = parts[1].split(':'),
                    choice = choices[index];

                return {
                    index: index,
                    choices: choices,
                    choice: choice
                };
            }

            //Main module definition.
            define({
                normalize: function (name, normalize) {
                    var parsed = parse(name),
                        choices = parsed.choices;

                    //Normalize each path choice.
                    for (i = 0; i < choices.length; i++) {
                        //Call the normalize() method passed in
                        //to this function to normalize each
                        //module ID.
                        choices[i] = normalize(choices[i]);
                    }

                    return parsed.index + '?' + choices.join(':');
                },

                load: function (name, require, load, config) {
                    require([parse(name).choice], function (value) {
                        load(value);
                    });
                }
            });

        }());

### Require
1. A local require passed to an AMD define factory function

        define(['require'], function (require) {
            //the require in here is a local require.
        });

        define(function (require, exports, module) {
            //the require in here is a local require.
        });

2. Can include require(String), require(Array, Function), require.toUrl(String)
## CommonJS and Node Modules
1. The node module system is roughly equivalent to the CommonJS spec
2. Common JS was a list group that began to define a common module system for js, out of which grew AMD and split off
3. CommonJS defines three locally scoped variables for injection into the module; exports, require, and module 
4. Node uses module.exports

        var customerStore = require('store/customer');
        var when = require('when');

        module.exports = function (id) {
            return when(id).then(customerStore.load);
        };

5. CommonJS basics

        // module app/mime-client
        var rest, mime, client;

        rest = require('rest');
        mime = require('rest/interceptor/mime');

        client = rest.chain(mime);

        // debug
        console.log(module.id); // should log "app/mime-client"

        exports.client = client;

6. In CommonJS there is no need for an IIFE or AMD's define. All of the var statements are scoped to the module as are require, exports, and module!
### require
1. Call require(id) for each needed module in the local scope. CommonJS uses 'packagename/moduleid' for requires where package name is the local directory name. 
### exports
1. exports is used as the public API of your module. All objects, functions, constructors, etc. of your module must be declared as properties of exports.
### module
1. Originally used to provide module metadata
### exports vs. module.exports
1. exports in CommonJS is an object literal that holds all of the functions, objects, etc. that the module provides. (It also allows circular dependencies)
### CommonJS in the browser
1. The browser cannot handle commonjs modules, so this requires a tool to generate a transport format like browserify or cram.js. curl.js does not require a build step
### node modules
1. Node modules work very similarly to commonjs but as noted use the module.exports.
2. One thing to be aware of is cyclic module dependencies because the processing order is complicated and can cause problems in the program output
3. core modules load from the node source library lib/ folder and are loaded as require('name'). File modules are in the file system and are loaded using an absolute file path as require('/name') with ./ making the file relative to the file calling require. Throws a MODULE_NOT_FOUND error if not found. 
4. Modules are cached after the first time they are loaded. 
5. module object - included in a module as a free variable referencing the current module
6. module.exports - desired export objects and functions are bound as properties of the module.exports object
## ES6 Modules
1. Use export and import like export and require. Import can import one or more variables from another module. 
2. Similar to CommonJS, they have a compact syntax, a preference for single exports and support for cyclic dependencies.
3. Similar to AMD, they have direct support for asynchronous loading and configurable module loading.
4. Their syntax is even more compact than CommonJS’s.
5. Their structure can be statically analyzed (for static checking, optimization, etc.).
6. Their support for cyclic dependencies is better than CommonJS’s.
7. Named exports - several per module

        //------ lib.js ------
        export const sqrt = Math.sqrt;
        export function square(x) {
            return x * x;
        }
        export function diag(x, y) {
            return sqrt(square(x) + square(y));
        }

        //------ main.js ------
        import { square, diag } from 'lib';
        console.log(square(11)); // 121
        console.log(diag(4, 3)); // 5

8. Default exports - one per module 

        //------ myFunc.js ------
        export default function () { ... };

        //------ main1.js ------
        import myFunc from 'myFunc';
        myFunc();

9. Benefits:
    - faster lookup - CommonJS uses dynamic object property lookup whereas ES6 imports are static and can be optimized.
    - variable checking - statically know all global variables, statically know module imports, and local variables are all contained within the module
    - ready for macros - still might be included in the future
    - ready for types - faster statically typed dialects of javascript such as LLJS which compiles to asm.js
    - both sync and async loading 
    - support for cyclic dependencies
## RequireJS
- RequireJS is a file and module loader optimized for in-browser use. RequireJS is an AMD loader i.e. it implements AMD. All definition examples below pretty much correspond to the AMD examples above
- Basics

        <script data-main="scripts/main" src="script/require.js"></script>

- The data-main tells require where to look for the main loader file that draws all of the dependencies of the application into the application but require does the work
- Simple definition 

        define({
            color: "black",
            size: "unisize"
        });

- Definition functions

        define(function () {
            //Do setup work here

            return {
                color: "black",
                size: "unisize"
            }
        });

- Definition functions with dependencies

        //my/shirt.js now has some dependencies, a cart and inventory
        //module in the same directory as shirt.js
        define(["./cart", "./inventory"], function(cart, inventory) {
                //return an object to define the "my/shirt" module.
                return {
                    color: "blue",
                    size: "large",
                    addToCart: function() {
                        inventory.decrement(this);
                        cart.add(this);
                    }
                }
            }
        );

- Configuration options

        <script src="scripts/require.js"></script>
        <script>
          require.config({
            baseUrl: "/another/path",
            paths: {
                "some": "some/v1.0"
            },
            waitSeconds: 15
          });
          require( ["some/module", "my/module", "a.js", "b.js"],
            function(someModule,    myModule) {
                //This function will be called when all the dependencies
                //listed above are loaded. Note that this function could
                //be called before the page is loaded.
                //This callback is optional.
            }
          );
        </script>

- Includes support for Web Workers, Multiversion modules, loading modules from packages, Rhino, Nashorn, and Error Handling. 
## Browserify
- Browserify is a tool for compiling node-flavored commonjs modules for the browser.
- npm and node packages can be used in the browser along with custom code
- follow the commonjs/node style with require, module.exports, and module
- client-side code is compiled into AMD compatible code, essentially works as a tool to translate the node commonjs style module system into the AMD style needed for the browser
- watchify will automatically rebuild file dependencies when changed
- use grunt-browserify for building with grunt
