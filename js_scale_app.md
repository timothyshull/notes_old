# Scalable JavaScript Application Architecture
- Large-scale JavaScript apps are non-trivial applications requiring significant developer effort to maintain, where most heavy lifting of data manipulation and display falls to the browser.
- Most applications need the following -
    - custom widgets
    - models
    - views
    - controllers
    - templates
    - libraries/toolkits
    - an application core
- Main questions this addresses - 
1. How much of this architecture is instantly re-usable?
2. How much do modules depend on other modules in the system?
3. If specific parts of your application fail, can it still function?
4. How easily can you test individual modules?
##Design Patterns
###Module Pattern
- Any significantly non-trivial app should be built from modular components
- Each module should have a relatively limited knowledge of/interaction with the rest of the system, in this case it happens through a mediator via a facade
- In JavaScript, there are many standards for module patterns (object literals, basic module pattern, CommonJS, AMD) and their value is in the encapsulation of privacy, state, and organization. The main concern is the API
- See notes on modules 
###Facade Pattern
- Used to reveal a simplified interface for an underlying complex body of code (hence facade)
- Many JS libraries present a limited abstraction of the variety of complex methods defined within the library
- Basic facade -

            var module = (function() {
                var _private = {
                    i:5,
                    get : function() {
                        console.log('current value:' + this.i);
                    },
                    set : function( val ) {
                        this.i = val;
                    },
                    run : function() {
                        console.log('running');
                    },
                    jump: function(){
                        console.log('jumping');
                    }
                };
                return {
                    facade : function( args ) {
                        _private.set(args.val);
                        _private.get();
                        if ( args.run ) {
                            _private.run();
                        }
                    }
                }
            }());
 
 
            module.facade({run: true, val:10});
            //outputs current value: 10, running

###Mediator Pattern
- The mediator is like an air traffic control tower - rather than modules communicating with modules, they communicate with a centralized controller which is key to complex coordination
- Mediators are used when the communication between modules may be complex, but is still well defined. If it appears a system may have too many relationships between modules in your code, it may be time to have a central point of control, which is where the pattern fits in.
- Mediators are like observers (pub/sub) for many modules at once (also similar to osx delegation and protocols or the adapter pattern)
- Benefits - decoupling, multiple message responses at once, easier to add or remove features
- Disadvantages - minor performance drop, harder to establish how the system is reacting to events

            var mediator = (function(){
                var subscribe = function(channel, fn){
                    if (!mediator.channels[channel]) mediator.channels[channel] = [];
                    mediator.channels[channel].push({ context: this, callback: fn });
                    return this;
                },
 
                publish = function(channel){
                    if (!mediator.channels[channel]) return false;
                    var args = Array.prototype.slice.call(arguments, 1);
                    for (var i = 0, l = mediator.channels[channel].length; i < l; i++) {
                        var subscription = mediator.channels[channel][i];
                        subscription.callback.apply(subscription.context, args);
                    }
                    return this;
                };
 
                return {
                    channels: {},
                    publish: publish,
                    subscribe: subscribe,
                    installTo: function(obj){
                        obj.subscribe = subscribe;
                        obj.publish = publish;
                    }
                };
 
            }());


            //Pub/sub on a centralized mediator
 
            mediator.name = "tim";
            mediator.subscribe('nameChange', function(arg){
                    console.log(this.name);
                    this.name = arg;
                    console.log(this.name);
            });
 
            mediator.publish('nameChange', 'david'); //tim, david
 
 
            //Pub/sub via third party mediator
 
            var obj = { name: 'sam' };
            mediator.installTo(obj);
            obj.subscribe('nameChange', function(arg){
                    console.log(this.name);
                    this.name = arg;
                    console.log(this.name);
            });
 
            obj.publish('nameChange', 'john'); //sam, john

##Abstraction of the core
- Acts as a facade for the modules to interface with the core (also known as sandbox controller)
##Application core
- The core acts as the mediator
- Tasks
    - Manages module lifecycle
    - Reacts to important events that affect the overall application
    - Should be able to easily adapt to added and removed modules
- Once a module starts, it should execute automatically without intervention from the core
##All Together
 - Modules should provide a meaningful unit of user interaction and trigger events based on interactions, but should not depend on other modules
 - The sandbox acts as the facade for the application, simplifying the interface for a global, providing security by checking that a module is allowed to broadcast the event that it is broadcasting
 - Application core is a mediator (pub/sub), responsible for start/stop module management and handling module failure
##Automatic Event Registration
- Wiring publishers to subscribers uses a messaging/notification system that is dependent on established naming conventions
- The setup for this pattern involves registering all components which might subscribe to events, registering all events that may be subscribed to and finally for each subscription method in your component-set, binding the event to it. It's a very interesting approach which is related to the architecture presented in this post, but does come with some interesting challenges.

#Details
##Modules

            Core.register("module-name", function(sandbox){  
                return {  
                    init: function(){  
                        //constructor  
                    },  
                    destroy: function(){  
                        //destructor  
                    }  
                };  
            });  

- Only call your own methods or those on the sandbox
- Don't access DOM elements outside of your box
- Don't access non-native global objects
- Anything else you need, ask the sandbox
- Don't create global objects
- Don't directly reference other modules
##Sandbox

            Core.register("module-name", function(sandbox){  
                return {  
                    init: function(){  
                        //not sure if I'm allowed...  
                        if (sandbox.iCanHazCheezburger()){  
                            alert("thx u");  
                        }  
                    },  
                    destroy: function(){  
                        //destructor  
                    }  
                };  
            });  

- consistent interface for modules
- security, limits which parts of a framework a module can access
- communication - translates module requests into core actions
##Core

            Core = function () {  
                var moduleData = {};  
                return {  
                    register: function (moduleId, creator) {  
                        moduleData[moduleId] = {  
                            creator: creator,  
                            instance: null  
                        };  
                    },  
                    start: function (moduleId) {  
                        moduleData[moduleId].instance =  
                        moduleData[moduleId].creator(new Sandbox(this));  
                        moduleData[moduleId].instance.init();  
                    },  
                    stop: function (moduleId) {  
                        var data = moduleData[moduleId];  
                        if (data.instance) {  
                            data.instance.destroy();  
                            data.instance = null;  
                        }  
                    }  
                }  
            } ();  

- Manage module lifecycle
- Tell modules when to start and stop doing their job
- Enable inter-module communication
- Allow loose coupling between modules that are related to one another
- General error handling
- Detect, trap, and report errors in the system
- Be extensible
- The first three jobs are not enough

            //register modules  
            Core.register("module1", function(sandbox){ /*...*/ });  
            Core.register("module2", function(sandbox){ /*...*/ });  
            Core.register("module3", function(sandbox){ /*...*/ });  
            Core.register("module4", function(sandbox){ /*...*/ });  
            //start the application by starting all modules  
            Core.startAll();  

##Base libraries
- Browser normalization
- Abstract away differences in browsers with common interface
- General-purpose utilities
- Parsers/serialises for XML, JSON, etc.
- Object manipulation
- DOM manipulation
- Ajax communication
- Provide low-level extensibility
###Goals
- Only the base library knows which browser is being used
- Only the application core knows which base library is being used
- Only the sandbox knows which application core is being used.
- Each module knows nothing except that the sandbox exists.
- And finally no single part of the web application knows about the web application.

#Design Patterns Used
##General Design Patterns
- Proxy [3] to impose strict rules on modules ; the sandbox objects act as proxies between user interface modules and the application core Facade
- Builder [19] to assemble the Sandbox API from separate parts, each defined by a separate Plug-in [20].
- Facade [4] to provide a single entry point to the methods part of the application core programming interface
- Mediator [5] to ensure loose coupling, all communication between user interface modules is channeled through the application core Facade. Through this link, modules send messages to the application core Facade, never to each other.
- Observer [6] mechanism provided by the application core Facade lets modules subscribe to events to get notified when these events occur. Each user interface module is an Observer of events broadcast by the application core Facade.
- Adapter [7] to translate messages from the objects used in the JavaScript application to the format used for communication with the server, and vice versa
##Design Patterns Specific to JavaScript
- JavaScript Module Pattern [8] to create a distinct scope and namespace for each module, while keeping the global scope clean of all but one global object
- Portlet [9] user interface modules designed as independent applications keeping track of their independent state
- Cross-Browser Component [10] as a strong foundation for the architecture, to abstract differences between browsers
#Sequence of Messages
- 1-6: each user interface module subscribes through the Sandbox to events published by the Application Core Facade (Proxy and Observer patterns)
- 7-9: one user interface module calls through the Sandbox one of the methods of the Application Core API, which triggers a corresponding processing in one application core module (Proxy, Mediator and Facade patterns)
- 10-11: the Application Core Module sends a request to the server, which is translated by the Adapter into a suitable format, and processed by a Cross-Browser Component (Adapter and Cross-Browser Component patterns)
- 12-13: the asynchronous response from the server is translated by the Adapter into a JavaScript object and provided to the Application Core Module for further processing. (Adapter and Cross-Browser Component patterns)
- 14: the Application Core Module publishes a new event on the Application Core Facade (Observer pattern) as a result of its processing
- 15-20: each user interface module that subscribed to this event gets notified through its Sandbox, and updates its display in a completely independent way (Proxy, Observer and Portlet patterns)

http://addyosmani.com/largescalejavascript/
https://www.microsoft.com/en-GB/developers/articles/scalable-javascript-application-architecture
https://eric-brechemier.github.io/lb_js_scalableApp/doc/javascript-application-design-patterns.pdf
https://github.com/addyosmani/aura/
http://scaleapp.org/readme.html#mediator
https://github.com/flosse/scaleApp
