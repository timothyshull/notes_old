##Ruby
###General
- minimal use of comments
- camel case for classes, snake case everywhere else
- Generally use parentheses even though they can be left out
- Use puts without parentheses
- Can include multiple statements on one line wiht ;
- {} code blocks are essentially the same as do - end i.e.

        10.times { |n| puts "The number is #{n}"}

    is equivalent to 

        10.times do |n|
            puts "The number is #{n}"
        end

- Fold to one line when it provides clarity, don't do it when 1 line is too long
- No need for return statements, Ruby returns the last evaluated expression
####Constructors
- s = "string" is equivalent to s = Sting.new(s), var = class instance or var = Class.new(classinstance)
- the superclass dictates the class hierarchy and inheritance
###Choose the Right Control Structure
- if is generally used like normal
- unless is like if + not
- can collapse

        if cond
            @var = var
        end

    to 
    
        @var = var if cond
    
    or 
    
        @var = var unless (while, until) cond

- Always use each in Ruby instead of for...in (because Ruby has built in methods which define the behavior of each statements when applied to different objects)
- Switch statements are expressed as case statements

        case cond
            when cond1
                exec
            when cond2
                exec
            else
                exec
        end

    can use assignment instead, i.e.
    
        var = case cond
    
    or
    
        var = case cond
            when cond1 then val1
            when cond2 then val2
            else val3
            end
    
    case uses ===
- Ruby only treats false and nil as false, 0 is true in Ruby
- Ruby also includes a ternary operator

        if_this_is_a_true_value ? then_the_result_is_this : else_it_is_this
    
    and
    
        @val = '' unless eval
    
    which is better written as 
    
        @val = eval || ''
    
###Smart collections
- Array literal

        var = [val1, val2, ..., valn]

- Array literal mass assignment with strings

        var = %w{str1 str2 str3 str4}

- Hashes (old, generalized method using string keys)
    
        var = {"key1" => val1, ... etc}

- Hashes (old method with symbols)

        var = {:sym1 => val1 ... etc }
    
- Hashes (new method with symbols)

        var = {sym1: val1 ... etc}

- One can define an argument with a default value using an arg = val in the parameter location of the definition

        def method( name, size = 12 )
          # Perform method
        end
    
- A * before an argument passed allows the argument to be any number of args (i.e. variable arity). The resulting *'d arguments variable is an array containing all of the extra arguments. Only allowed one starred parameter

        def echo_all( *args )
          args.each { |arg| puts arg }
        end

- Use each for iterating through arrays and hashes!

        array.each { |elem| puts elem }

- pp is a pretty printer that can be used with collections instead of puts
- Array contains a number of methods including map, inject, find_index, etc. 
- Some methods change the collection, some leave undisturbed (i.e. reverse returns a reversed copy of the collection)
- Hashes are ordered!
- xmlsimple gem converts XML to a hash
- Do not change array or hash w/map and each
- ! on methods can modify the original collection 
###Regex
- Letters and numbers match themselves
- Case sens by default
- . mathces any single character
- \ turns off special meanings for characters
- [] sets a range of chars to match, can be specified individually or over range, i.e. 1-9 or A-Z
- \d - match digit
- \w - match any letter, number, or underscore
- \s - match any whitespace
- | or A|B match A or B
- * - matches any number of preceding char
- .* will match any number of anything
- To make Ruby Regexp, encase in / /
- =~ returns index of match, nil of none, Regexp can be on either side
- \A to look for string at start
- (Time + parse standard format date/time strs)
###Strings
- '' or ""and use #{} for string interpolation (only in "")
- %q{} or %q() or any special character for delimiter
- can span lines with here doc -> cvar = <<EOF lines...lines EOF
- methods - lstrip, rstrip, chomp, chop, sub (string replacement), gsub for many replacements, split
- use ! on methods to modify the string
- index returns location of substringstrings bot calls of chars and bytes - each-char, each-byte, each-line but no each
- inlfection, html-escape, gsub to create string manipulation w/ ease
- strings and collections are mutable!
- preferable to say var= var.method over var.method!
- can use ranges to index into strings
###Symbols
- Used for the "stands for" job of strings, i.e. mainly a key or representation
- Lacks methods for processing
- There is only ever one instance of a distinct symbol within a namespace
- Symbols are immutable - to_s or to_sym
- *All objects contain a method public_methods which returns all public methods on that object, now it returns an array of symbols of the methods
- cannot start with a number
###Functions/methods
- in Ruby you can choose to not use () for function calls

        func (param1, param2)
    
    or 
    
        func param1, param2
    
- When a hash is the final parameter in a function, the containing {} are optional

        func (param1, hash)

    is equivalent to 
    
        func param1, {hash}
    
    or
    
        func param1, hash

- However, in the preceding situation, and anywhere a a symbol uses hyphens, the hash key/value pair must be denoted as :key-1 => value and not key-1: value
###Class definition
- basic definition

        class User
            attr_accessor :sym1, :sym2
    
            def initialize(attributes = {})
                @sym1 = attributes[:sym1]
                @sym2 = attributes[:sym2]
            end
    
            def method
                @sym1 = something_else
            end
    
            private
        
                def private_method
                end
        end

- initialize methods must be used to set instance variables
- variable types - local variables - defined beginning with a lowercase letter or _, only available within a method. Instance variables - defined using @, available across methods for a particular instance of an object, and change from object instance to object instance. Class variables - defined using @@, available across different objects of a type, not duplicated with specific object instantiation. Global variable - used for a single variable across classes, use $ to define


##General Web
###Cookies 
- Session cookie - destroyed on browser close
- Persistent cookie - used to remember users
- Each type can be created using JavaScript, PHP, HTTP, or Ruby
- Max size is generally 4096 bytes
###Sessions
- Used to avoid large data stored in cookies
- store a session id in the cookie
- Uses the ID (generally encrypted) to check if the session is active, query the database for the requested information, and return the data to the requested site
###Local storage
- Primarily used to store stuff on the client side and access on the client side (as opposed to cookies which generally store server side information on the client side)
- Generally 5-10 MB but can be increased, signs depends on browser and UA settings
###Caching
- Reduces latency by storing resources from a website for a specified length of time 
- Reduces network traffic
- Can be controlled through HTML meta tags and HTTP headers

##JavaScript
###Objects
- literals can be assigned using {} and name value pairs separated by commas for pairs and delineated using a colon between the name and value
- retrieval can be done using the . notation or ["name"], assignment is done in the same way but using = and value
- objects are always passed by reference
####Prototypes
- objects all inherit from prototypes
- can simplify the construction of an object by adding a constructor function to the global object for that object type
####Reflection
- use the typeof operator to test the type of method (data type or function)
- hasOwnProperty - returns true if the inspected property has properties of its own, generally false for functions
####Enumeration
- use for...in loops over properties of an object to test the properties. No guarantee of the order, it is best to make an array of the properties
####Delete
- removes a specified property from an object
- sometimes, through prototypal inheritance, a property of the same name will then show through from higher in the prototype chain
####Global Abatement
- use var myapp = {}; as a container to add objects and methods to (within an application) to reduce cluttering of the global namespace, potential name collisions, and other possible errors
###Functions
- programming is generally the act of factoring a set of requirements into a set of functions and data structures
- functions are objects in JavaScript
- function.prototype contains a link to the context and a link to the code implementing behavior
- function literal

        var myvar = function (params) {definition};

- a function declaration defines a named function without variable assignment
- a function expression defines a function as part of a larger expression syntax, generally assigning the function to a variable. The assigned function can be named or anonymous
- Hoisting - defines a functions context upon interpretation, pulls vars to the top as undefined, pulls function declarations to the top, assigns any vars, including vars w/function defs, evaluates executables
- Invocation - () is the invocation operator, follows any expression that causes a function to evaluate
- a function invoked with too many or too few args simply drops the excess arguments or adds the missing arguments as undefined
- method invocation - a method is a function stored as a property of an object
- invoked using . or ["method"] and () 
##CSS
###Attributes for grid design
- float
- position
- display -> inline, block, etc
- width & height
- margin (auto to center)
- max-width
###Box sizing
- Makes the calculation of feature sizes more intuitive

        * {
            -webkit-box-sizing: border-box;
            -moz-box-sizing: border-box;
            box-sizing: border-box;
        }

- content-box: default, width & height are measured using only the content, padding border & margin are outside of the box.
- padding-box: width & height include the padding but not the border or margin
- border-box: width and height include the padding and the border but not the margin
###Position
- static - default, current position in flow
- relative - starts at static relative to previous, then changes depending on top, right, bottom, left. Leaves a gap where the element should have been. (change in position from default position)
- absolute - position element at specified position relative to closest positioned ancestor or the containing block (do not leave space). If you define both a top and bottom it will always stretch relative to the screen size (change in position from parent)
- fixed - position relative to viewport (do not leave space) (change in position from viewport as parent). Top property takes precedence for position
- sticky - relative to normal flow, offset to flow root, but does not affect the position of any of the following boxes
###Float
- taken from normal flow and positioned to left or right of container and text and inline elements will wrap around, modifies display value, implies block
###Clear
- specifies whether an element can wrap around or exist next to floated elements or must be moved down (cleared) below them. With non-floats, moves the border edge down until it is below the margin element of all relevant floats. With floats, it moves the margin edge below the margin edge of other floated elements.
###Clearfix
- To make a container contain all floating elements within it either float the element also or use the ::after pseudo element, as in the clearfix

        #container:after { 
           content: "";
           display: block; 
           clear: both;
        }

###Display
- none - turns off display, doc is rendered as if elem does not exist, applies to all descendants
- inline - displayed in same line within containing element, only displayed as an individual block if between two other block elements
- block - generates a block element box with no other elements to either side unless specified to do so with floats
- inline-block - displays elements inline but they can retain their block level characteristics and can be contained within the parent container similar to floats but without the issues of floats (can set width and height manually) have to use alignment to avoid defaulting to bottom alignment. Inline block elements are also aware of the whitespace used in the markup.
####Block elements
- div, form, p, header, footer, section
####Inline elements
- span
####Flex
- gives the container the ability to alter the display of the contained items depending on the space needed. The container takes certain properties (display: flex, flex-direction, flex-wrap, justify content, align-items (vertical alignment), align-content (horizontal alignment)) and the children take certain properties (order, flex-grow, flex-shrink, flex-basis (default size before flex action), flex, align-self)
###Border
- border style
- border-width
- border-color
- or border: width style color
###Overflow
- defines behavior for how to display content that overflows its container.
- visible, hidden, scroll, auto, inherit
###Font-size
- Relative descriptions - small, medium, large, larger, smaller
- Pixels - 12px
- em - this defines the font size relative to the parent, dependent on at least one ancestor being defined in px. Generally the default is 16px, so a child of 1em will be 16px
- percentage - similar to ems but stated in percentage values
###@Media
- Queries the displaying media for properties and adjusts nested CSS properties dependently
- Types - screen, print, speech, all
- Features - width, height, aspect-ratio, orientation, resolution, scan, grid, update frequency, etc. 
###Z-index
- specifies z-order or visual overlap for elements, i.e. stack level
###Selectors
- Can shape cascade with selectors by styling and adding and removing elements
- type - comes from the element type of the base HTML
- class - allows styling rules on multiple elements using .class 
- id - allows unique element styling using #id
- descendant selector - type sub-type {}
- direct child - type > child {}
- sibling - type ~ sibling {}
- adjacent sibling - type + sibling {}
- attr & attr = - type[attr = ""] {}
- select all elements - * {}
- select all elements inside an element of type - type * {}
- begins with - ^ selector el[att^="val"] Matches any element "el" with an "att" attribute that begins with "val".
- ends with - $ selector - el[att$="val"] Matches any element "el" with an "att" attribute that ends with "val".
- contains - * selector - el[att*="val"] Matches any element "el" with an "att" attribute that contains the substring "val"
####Pseudo classes
- type:link
- :visited
- :enabled
- :hover
- :focus
- :active
- :disabled
- :checked 
- :indeterminate
#####Structural and Positioning
- :first-child
- :last-child
- :only-child
- :first-of-type
- :last-of-type
- :only-of-type
#####Negation Pseudo class
- type:not(.class)
###Optimization
- Style your architecture depending on the need of the application
    - Separate by use - base, components, modules (partials)
    - Object oriented - separate structure from skin, separate content from container, all elements should look the same separate as together
    - Scalable and modular
        - base layout, module, state, theme
    - Don't group rules, slows render time
    - Use classes, combine and reuse
    - Use key selectors (selector furthest to right) to correspond to page load
    - Pair selectors, separate with commas
    - gzip using .htaccess
    - Image compression, set height and width, 
    - Concatenate and minify
###Preprocessors
- Sass, LESS, scss
- Main facilities - allows variables, mixins, and nesting of rules
- Sass is whitespace sensitive, scss builds on Sass, allowing facilities but requiring ;, {}, etc (i.e. not whitespace sensitive)
###Units
50% - percentage
400px - pixels (device pixels)
20em - relative unit
20rem - root em
15vw - viewport width
60vh - viewport height
60vmin - smaller of vw or vh
60vmax - larger of vw or vh
4in - inches
20cm - centimeters
200mm - millimeters
120pt - points
40pc - picas
60ex - x-height
60ch - based on width of zero (0) character

6.1. Angle Units: the <angle> type and deg, grad, rad, turn units

Angle values are dimensions denoted by <angle>. The angle unit identifiers are:

deg
Degrees. There are 360 degrees in a full circle.
grad
Gradians, also known as "gons" or "grades". There are 400 gradians in a full circle.
rad
Radians. There are 2π radians in a full circle.
turn
Turns. There is 1 turn in a full circle.
For example, a right angle is 90deg or 100grad or 0.25turn or approximately 1.57rad.

6.2. Duration Units: the <time> type and s, ms units

Time values are dimensions denoted by <time>. The time unit identifiers are:

s
Seconds.
ms
Milliseconds. There are 1000 milliseconds in a second.
Properties may restrict the time value to some range. If the value is outside the allowed range, the declaration is invalid and must be ignored.

6.3. Frequency Units: the <frequency> type and Hz, kHz units

Frequency values are dimensions denoted by <frequency>. The frequency unit identifiers are:

Hz
Hertz. It represents the number of occurrences per second.
kHz
KiloHertz. A kiloHertz is 1000 Hertz.
For example, when representing sound pitches, 200Hz (or 200hz) is a bass sound, and 6kHz (or 6khz) is a treble sound.

6.4. Resolution Units: the <resolution> type and dpi, dpcm, dppx units

Resolution units are dimensions denoted by <resolution>. The resolution unit identifiers are:

dpi
dots per inch
dpcm
dots per centimeter
dppx
dots per px unit

RGB Color Values

em { color: #f00 }              /* #rgb */
em { color: #ff0000 }           /* #rrggbb */
em { color: rgb(255,0,0) }
em { color: rgb(100%, 0%, 0%) }

RGBA

em { color: rgb(255,0,0) }      /* integer range 0 - 255 */
em { color: rgba(255,0,0,1)     /* the same, with explicit opacity of 1 */
em { color: rgb(100%,0%,0%) }   /* float range 0.0% - 100.0% */
em { color: rgba(100%,0%,0%,1) } /* the same, with explicit opacity of 1 */

HSL

{ color: hsl(0, 100%, 50%) }   /* red */
{ color: hsl(120, 100%, 50%) } /* lime */ 
{ color: hsl(120, 100%, 25%) } /* dark green */ 
{ color: hsl(120, 100%, 75%) } /* light green */ 
{ color: hsl(120, 75%, 75%) }  /* pastel green, and so on */

Also includes certain color keywords

(ACID - atomic, consistent, isolated, durable)
##Internet Protocols and Data Communication
###HTTP
- % hex encoding
- date/time works from UTC or Greenwich mean time
- HTTP is stateless, i.e. no memory
- HTTP 1.1 allows persistent connections & HTTP pipelining, byte serving
- Header values (maps to collection of values in 100s)
    1. Info
    2. Success
    3. Redirect
    4. Client Error
    5. Server Error
- PUT is complete update
- PATCH is a new method added to the protocol in 2010
- HTTP 1.1 defines
    - The POST method is used to request that the origin server accept the entity enclosed in the request as a new subordinate of the resource identified by the Request-URI in the Request-Line
    - I.e. POST is used to CREATE
    - The PUT method requests that the enclosed entity be stored under the supplied Request-URI. If the Request-URI refers to an already existing resource, the enclosed entity SHOULD be considered as a modified version of the one residing on the origin server. If the Request-URI does not point to an existing resource, and that URI is capable of being defined as a new resource by the requesting user agent, the origin server can create the resource with that URI."
    - I.e. PUT is either CREATE or UPDATE
- Simply POST is used to modify and update a resource
- Simply PUT is used to create a resource or overwrite it
- PUT is idempotent POST is not idempotent
###HTTP Request Codes
- OPTIONS - request for information
- GET - retrieve URI and related data/resources
- HEAD - GET minus the body, just get full headers
- POST - update i.e. annotate existing resource, post to message board, provide data from form for a data handling process, append to a database
- PUT - create new entity and store under existing URI or replace existing resource
- DELETE - request that serve remove existing resource at URI
- TRACE - used for diagnostics
- PATCH - added in 2010, requests that entity at URI reflect changes requested in message
- PUT supplies full version of modified resource and requests replacement, PATCH contains instructions for how an existing resource should be modified
###REST
- set of software architecture constraints to formalize and simplify communication intended to increase reliability while reducing complexity
    - client-server model
    - stateless
    - cacheable
    - layered i.e. trail of requests is hidden to the user
    - code on demand i.e. executables are sent from server to client upon request as a resource, i.e. JavaScript script document
    - Uniform interface 
        - URI's correspond to individual resources
        - Client can manipulate the served resources
        - Each message also contains information on how to process the resource, i.e. MIME
        - Served resources represent actions through representations i.e. URI's represent a logical correspondence to the resource
    - Representation - client's request state
    - Transfer - sending of state from server to client
    - Partial updates RESTfulness is a topic of debate
    - URI's are the vehicle for transition of state
- GET - no side effects, retrieval is not equal to change, list URI of collection or retrieve a specific element or resource
- PUT - Replace or create the collection or the element, this is idempotent meaning that N requests is equivalent to 1 request
- POST - create new member in collection/element
- DELETE - delete collection or element
- (Rails translates these differently which is also a topic of debate. CRUD goes to POST, GET, PATCH, DELETE, some argue for POST, GET, POST, POST)
###Other Protocols
- TCP/IP - Transmission control protocol/internet protocol - Internet protocol suite
- Application layer - SMTP, FTP, SSH, HTTP, + more
- Transport layer - UDP - User datagram protocol - basic, TCP - control flow
- Internet Layer - IP for routing (includes DNS), IPv4, IPv6, ICMP, errors
- Link Layer - local network topology
- Basic network topology - Host -> Router <- & -> Router -> Host
###TCP
- delivers octets, paired sequence numbers followed by client acknowledgement, includes a checksum to ensure data integrity
- TCP Header - src port, dest port, zeros, protocol, length, datagrams/buffer are tagged w/header, limit 64Kb but changes with transmission medium
###UDP
- does not check delivery order or duplicate protection
- sends info as header, 4 octets, src port, dest port, length, checksum, then data.
- differs depending on IPv4 or IPv6
###CRUD
- Create - PUT/POST - INSERT
- Read - GET - SELECT
- Update - PUT/PATCH - UPDATE
- Dstroy - DELETE - DELETE
###Server concepts
- Server - any application that accepts connections to service requests
- Proxy - intermediary server + client (must function as both to be a proxy)
- Gateway - intermediary server for requests only
- Tunnel - transparent intermediary, blind relay 
###Full Stack Layers
- Front end
    - Business needs of customer
    
    - UX - the application should just work; built on simplification and efficiency, how does it interact with the user when something goes wrong
    
    - UI - VIEW - readable layout, visual design with HTML, CSS, JavaScript for change and interaction, data organization for user
    
    - Internet
    
    - Router - sits behind layers provided by the server to route incoming requests for a web application
    
    - API Layer/Action Layer - CONTROLLER - functions as a go-between for the world and data storage on in the server infrastructure, heavy usage of frameworks, simple, clear, consistent programming interfaces
    
    - Business Logic - MODEL - object orientation within the application programming, heart of the value for the application, built on frameworks
    
    - Data modeling - foundation for the business logic, determined by structure of database, controls data persistence, normalized relational model: foreign keys, indexes, look-up tables, non-relational vs. relational
    
    - Server, network, and hosting - storage, filesystem, data redundancy and hardware for data persistence, scaling (hardware has needs and constraints), multithreading, process management and spawning, data access control for threads (avoid race conditions), DevOps (development operations)
- Back end
###Server/Application Examples
- Application sits behind the app server which sits behind the server process. The app server and server process forward requests back and forth to each other. The server handles incoming requests from the internet (maybe interpreted by another server application) including HTTP, FastCGI, SCGI, AJP
- The server sits directly on top of the application, forwarding requests directly to the application and receiving responses and requests directly from the application (think PHP on Apache)
- The application is the server (Node and Meteor applications can be thought of this way
- All of the above examples generally sit behind a proxy and/or layers of proxies, tunnels, and gateways
- Forward proxy
        client <- -> proxy <- -> server
- Reverse proxy
        server <- -> proxy <- -> net <- -> client
###Simplified processing model
- Serial - -> -> -> ->
- Multiplexed
    ->    ->    ->
       ->    ->    ->
- Multithreaded
    process 1 - ->    ->    ->
    process 2 -    ->    ->    ->


