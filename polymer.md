# Polymer
- Thin API on top of web components
## Element declaration
- wrap a polymer element in <polymer-element> with a tag name and a constructor (opt)

        <polymer-element name="tag-name" constructor="TagName">
          <template>
            <!-- shadow DOM here -->
          </template>
          <script>
            Polymer({
              // properties and methods here
            });
          </script>
        </polymer-element>

- name specifies the name of the custom element
- template defines markup that is cloned into the shadow dom (opt) (must come before script)
- inline script registers the element with Polymer which allows it to be recognized as a custom element
## Attributes
- name required
- attributes optional - used to publish properties (see below)
- extends 
## Published Properties
- a published property name is the elements public API
  - support for two-way, declarative data binding
  - Declarative initialization with HTML attribute and matching name
  - The current val of a property can be reflected back to an attribute with a matching name
- Two ways - include in polymer element's attributes or add a publish object to the object prototype, use the attributes method

        <polymer-element name="x-foo" attributes="foo bar baz">
          <script>
            Polymer();
          </script>
        </polymer-element>
  
      <polymer-element name="x-foo">
        <script>
          Polymer({
            publish: {
              foo: 'I am foo!',
              bar: 5,
              baz: {
                value: false,
                reflect: true
              }
            }
          });
        </script>
      </polymer-element>

- baz allows attribute reflection - this.name = "Joe" = this.setAttribute('name', 'Joe')
- by default attributes are initialized to undefined, can set these ways 

        <polymer-element name="x-foo" attributes="bar">
          <script>
            Polymer({
              // x-foo has a bar property with default value false.
              bar: false
            });
          </script>
        </polymer-element>
    
        <polymer-element name="x-foo">
          <script>
            Polymer({
              publish: {
                bar: false
              }
            });
          </script>
        </polymer-element>
    
- for property values that are objects or arrays you set them in the created callback

        <polymer-element name="x-default" attributes="settings">
          <script>
            Polymer({
              created: function() {
                // create a default settings object for this instance
                this.settings = {
                  textColor: 'blue'
                };
              }
            });
          </script>
        </polymer-element>

- attributes are a way to configure an element declaratively - options for configuration

        <x-hint count="7"></x-hint>
        
        <x-name fullname='{ "first": "Bob", "last": "Dobbs" }'></x-name>
        
        xname.fullname = { first: 'Bob', last: 'Dobbs' };
        
        <polymer-element name="hint-element" attributes="isReady items">
          <script>
            Polymer({

              // hint that isReady is a Boolean
              isReady: false,

              publish: {
                // hint that count is a Number
                count: 0
              },

              created: function() {
                // hint that items is an array
                this.items = [];
              }
            });
          </script>
        </polymer-element>

- published properties can be data-bound

        <polymer-element name="name-tag" attributes="name nameColor">
          <template>
            Hello! My name is <span style="color:{{nameColor}}">{{name}}</span>
          </template>
          <script>
            Polymer({
              nameColor: "orange"
            });
          </script>
        </polymer-element>

### Computed Properties
- dynamic and computed based on other property values, defined in computed object on the element's prototype

<link rel="import"
      href="../bower_components/polymer/polymer.html">

        <polymer-element name="square-element" attributes="square">
          <template>
            <style>input {box-sizing:border-box;width:100%;}</style>
            <input type="number" value="{{num}}"><br>
            <em>{{num}}^2 = {{square}}</em>
          </template>
          <script>
            Polymer('square-element', {
              num: 2,
              computed: {
                square: 'num * num'
              }
            });
           </script>
        </polymer-element>


        <!DOCTYPE html>
        <html>
          <head>
            <script src="webcomponents.min.js"></script>
            <link rel="import" href="square-element.html">
          </head>
          <body>
            <square-element></square-element>
          </body>
        </html>

- Returns a value box that takes input and prints the result below
- computed properties are read-only
### Declarative event mapping
- on-event binds event to method in component

        <polymer-element name="g-cool" on-keypress="{{keypressHandler}}">
          <template>
            <button on-click="{{buttonClick}}"></button>
          </template>
          <script>
            Polymer({
              keypressHandler: function(event, detail, sender) { ...},
              buttonClick: function(event, detail, sender) { ... }
            });
          </script>
        </polymer-element>

- cant put executables in the attribute
- event names are lowercase
- event handler passed following args - 
  - inEvent - standard event object
  - inDetail - inEvent.detail
  - inSender - node that declared handler (diff from inEvent.target (lowest node receiving event) and inEvent.currentTarget (the component processing the event)
- can also add imperatively

        <polymer-element name="g-button">
          <template>
            <button>Click Me!</button>
          </template>
          <script>
            Polymer({
              eventDelegates: {
                up: 'onTap',
                down: 'onTap'
              },
              onTap: function(event, detail, sender) {
                ...
              }
            });
          </script>
        </polymer-element>

### Automatic node finding
- elements in polymer element shadow dom tagged with an id can be found in the this.$ hash, i.e. id="nameInput" is this.$.nameInput

        <polymer-element name="x-form">
          <template>
            <input type="text" id="nameInput">
          </template>
          <script>
            Polymer({
              logNameValue: function() {
                console.log(this.$.nameInput.value);
              }
            });
          </script>
        </polymer-element>

- can use querySelector from known elements in shadow dom to retrieve descendants
### Observing properties
- all polymer element properties can be watched for changes using the propertyName|Changed| handler, i.e.

        <polymer-element name="g-cool" attributes="better best">
          <script>
            Polymer({
              better: '',
              best: '',
              betterChanged: function(oldValue, newValue) {
                ...
              },
              bestChanged: function(oldValue, newValue) {
                ...
              }
            });
          </script>
        </polymer-element> 

- sometimes need observe, this allows mapping of observation to multiple properties, invoking the same callback on changes to either property, can use the this.$.property to observe a property on an element with an id
### Firing custom events
- use fire() to send custom events

        <polymer-element name="ouch-button">
          <template>
            <button on-click="{{onClick}}">Send hurt</button>
          </template>
          <script>
            Polymer({
              onClick: function() {
                this.fire('ouch', {msg: 'That hurt!'}); // fire(type, detail, targetNode, bubbles?, cancelable?)
              }
            });
          </script>
        </polymer-element>

        <ouch-button></ouch-button>

        <script>
          document.querySelector('ouch-button').addEventListener('ouch', function(e) {
            console.log(e.type, e.detail.msg); // "ouch" "That hurt!"
          });
        </script>

- nested elements can use custom on-* handlers
### Dealing with async
- changes are batched
- use async(), similar to setTimeout but automatically bound to the caller's this and and set a requestAnimationFrame value
- can still use setTimeout to delay events
- use the job() utility in this case

## Data Binding
- model is always the element itself

        <polymer-element name="name-tag">
          <template>
            This is <b>{{owner}}</b>'s name-tag element.
          </template>
          <script>
            Polymer('name-tag', {
              // initialize the element's model
              ready: function() {
                this.owner = 'Rafael';
              }
            });
          </script>
        </polymer-element>

- changing the value in javascript changes the display

        document.querySelector('name-tag').owner = 'June';

### template element
- template contents are hidden and they don't cause resources to be loaded or scripts to be run
  - in Polymer, the template defines the shadow dom
  - can use templates with data binding to render dynamic content
  
      <polymer-element name="greeting-tag">
        <!-- outermost template defines the element's shadow DOM -->
        <template>
          <ul>
            <template repeat="{{s in salutations}}">
              <li>{{s.what}}: <input type="text" value="{{s.who}}"></li>
            </template>
          </ul>
        </template>
        <script>
          Polymer('greeting-tag', {
            ready: function() {
              // populate the element’s data model
              // (the salutations array)
              this.salutations = [
                {what: 'Hello', who: 'World'},
                {what: 'GoodBye', who: 'DOM APIs'},
                {what: 'Hello', who: 'Declarative'},
                {what: 'GoodBye', who: 'Imperative'}
              ];
            }
          });
        </script>
      </polymer-element>

- Adding external javascript
  - greeting-tag.html

        <polymer-element name="greeting-tag">
          <!-- outermost template defines the element's shadow DOM -->
          <template>
            <ul>
              <template repeat="{{s in salutations}}">
                <li>{{s.what}}: <input type="text" value="{{s.who}}"></li>
              </template>
            </ul>
            <button on-click="{{updateModel}}">Update model</button>
          </template>
          <script src="greeting-tag.js"></script>
        </polymer-element>

  - greeting-tag.js
  
        Polymer('greeting-tag', {
          ready: function() {
            this.salutations = [
              {what: 'Hello', who: 'World'},
              {what: 'Goodbye', who: 'DOM APIs'},
              {what: 'Hello', who: 'Declarative'},
              {what: 'Goodbye', who: 'Imperative'}
            ];
            this.alternates = ['Hello', 'Hola', 'Howdy'];
            this.current = 0;
          },     
          updateModel: function() {
            this.current = (this.current + 1) % this.alternates.length;
            this.salutations[0].what = this.alternates[this.current];
          }
        });
  
  - index.html
  
        <!DOCTYPE html>
        <html>
          <head>
            <script src="webcomponents.min.js"></script>
            <link rel="import" href="greeting-tag.html">
          </head>
          <body>
            <greeting-tag></greeting-tag>
          </body>
        </html>

### Event handling and data binding
- add event handlers with declarative event mappers

        <template>
          <ul>
            <template repeat="{{s in stories}}">
              <li on-click="{{selectStory}}">{{s.headline}}</li>
            </template>
          </ul>
        </template>

        selectStory: function(e, detail, sender) {
          var story = e.target.templateInstance.model.s;
          console.log("Clicked " + story.headline);
          this.loadStory(story.id); // accessing non-rendered data from the model
        }

### Types of bindings
- create single instance, specifying a single object using bind
- create multiple instances of a template using repeat with an array of objects
- conditionally create an instance of a template
#### Single instance
- bind attribute

        <template>
          <template bind="{{person}}">
            This template can bind to the person object’s properties, like
            {{name}}.
          </template>
        </template>

- name is bound to person's name property
- named scope

        <template>
          <template bind="{{person as p}}">
            This template uses a named scope to access properties, like
            {{p.name}}.
          </template>
        </template>

#### Iterative
- single template instance for each item in an array

        <template>
          <template repeat="{{array}}">
            Creates an instance with {{}} bindings  for every element in the array collection.
          </template>
        </template>

- index value for each array item is accessible like this

        <template>
          <template repeat="{{user, userIndex in users}}">
            <template repeat="{{userFile, userFileIndex in user}}">
              {{userIndex}}:{{userFileIndex}}.{{userFile}}
            </template>
          </template>
        </template>

- can access both the array and its elements like this

        <template>
          <template bind="{{items}}">
            // {{length}} evaluates as items.length
            <p>Item count: {{length}}</p>
            <ul>
            <template repeat>
              // {{name}} here evaluates as the name of a single item
              <li>{{name}}</li>
            </template>
            </ul>
          </template>
        </template>

#### Conditional Templates

        <template>
          <template if="{{conditionalValue}}">
            Binds if and only if conditionalValue is truthy.
          </template>
        </template>

#### Importing templates by reference
- reuse by reference

        <template>
          <template id="myTemplate">
            Used by any template which refers to this one by the ref attribute
          </template>

          <template bind="{{}}" ref="myTemplate">
            When creating an instance, the content of this template will be ignored,
            and the content of # myTemplate is used instead.
          </template>
        </template>

- must use bind or repeat attribute along with the ref attribute to activate binding on the template

#### Recursive and dynamic templates
- can also use ref to create recursive templates 

        <template>
          <ul>
            <template repeat="{{items}}" id="t">
              <li>{{name}}
                <ul>
                  <template ref="t" repeat="{{children}}"></template>
                </ul>
              </li>
            </template>
          </ul>
        </template>

- assumes data structure like this 

        items: [
          {
            name: "1",
            children: [
              {
                name: "1.1",
                children: []
              }, {
                name: "1.2",
                children: [
                  {
                    name: "1.2.1"
                    children: []
                  }
                ]
              }
            ]
          }
        ]

- can bind nodes in nodes 

        <template repeat="{{node in nodes}}">
          <template bind="{{}}" ref="{{node.nodeType}}"></template>
        </template>

### Node bindings


#### Binding to text 

        <p>This paragraph has some {{adjective}} text.</p>

#### Binding to attributes

        <span style="color: {{myColor}}">Colorful text!</span>

- The way this works depends on the element being bound
  - most standard elements create a one-way binding
  - the form elements input, option, select, and textarea support two-way binding for certain elements
  - Polymer elements support two-way bindings for published properties

#### Binding to input elements
- input element: value and checked attributes.
- option element: value attribute.
- select element: selectedIndex and value attributes.
- textarea element: value attribute.

#### Binding to Polymer published properties
- say hello publishes the name property

        <polymer-element name="say-hello" attributes="name">
          <template>
            Hello, <b>{{name}}</b>!
          </template>
          <script>
            Polymer('say-hello', {
              ready: function() {
                this.name = 'Stranger'
              }
            });
            </script>
        </polymer-element>
        <polymer-element name="intro-tag" noscript>
          <template>
            <!-- bind yourName to the published property, name -->
            <p><say-hello name="{{yourName}}"></say-hello></p>
            <!-- bind yourName to the value attribute -->
            <p>What's your name? <input value="{{yourName}}" placeholder="Enter name..."></p>
          </template>
        </polymer-element>

        <intro-tag></intro-tag>
  
#### Binding objects and arrays to published properties

        <polymer-element name="name-tag" attributes="person">
          <template>
            Hello! My name is <span style="color:{{person.nameColor}}">
            {{person.name}}</span>
          </template>
          <script>
            Polymer('name-tag', {
              created: function() {
                this.person = {
                  name: "Scott",
                  nameColor: "orange"
                }
              }
            });
          </script>
        </polymer-element>

        <polymer-element name="visitor-creds">
          <template>
            <name-tag person="{{person}}"></name-tag>
          </template>
          <script>
            Polymer('visitor-creds', {
              created: function() {
                this.person = {
                  name: "Scott2",
                  nameColor: "red"
                }
              }
            });
          </script>
        </polymer-element>

#### Boolean attributes
- attribute?={{boolean-expression}}
- <span hidden?="{{isHidden}}">This may or may not be hidden.</span>
#### One-time bindings
- not dynamic
- use [[]] instead of {{}}, increase performance

## Helper methods
### Dynamic imports

        <html>
          <head>
            <script src="components/webcomponentsjs/webcomponents.min.js"></script>
            <link rel="import" href="components/polymer/polymer.html">
          </head>
          <body>
            <dynamic-element>
              I'm just an unknown element.
            </dynamic-element>

            <button>Import</button>

            <script>
              var button = document.querySelector('button');
              button.addEventListener('click', function() {
                Polymer.import(['dynamic-element.html'], function() {
                  document.querySelector('dynamic-element').description = 'a dynamic import';
                });
              });
            </script>
          </body>
        </html>
