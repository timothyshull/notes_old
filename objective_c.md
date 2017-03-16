# Objective-C
1. Superset of C, influenced by SmallTalk
2. Inherits syntax, primitive types, and flow control of C
3. Adds syntax for defining classes and methods
## At a Glance
### Apps are networks of objects
- OSX and iOS is working with objects from both frameworks and programmer defined
- Start with the public interface (header file) where you declare the public and private methods and data, and declare the types of messages (input and output) they send and receive
- Declare implementation in code files (m, c, cpp, mm) i.e. define executable code for the methods when they are called
### Categories extend existing classes
- Modify existing classes using categories
### Protocols define messaging contracts
- Protocols are used to define groups of methods that aren't tied to a specific class (think helper methods). A class can adapt a protocol but then has to define the implementation of the interface to the methods provided by the protocol
### Values and collections are usually objects
- Cocoa and cocoa touch classes are often used to represent values, i.e. NSString, NSNumber, NSValue
- Can instead use the primitive values from C such as int, float or char
- Collections are usually represented as one of the collection classes, i.e. NSArray, NSSet, or NSDictionary
### Blocks simplify common tasks
- Similar to closures, encapsulate captured state and implement a unit of work

        ^{
                 NSLog(@"This is a block");
            }

- Can declare variables to keep track of blocks

        void (^simpleBlock)(void);

- Can refer to a simple block that takes no arguments and returns no values using the following syntax

        simpleBlock = ^{
                NSLog(@"This is a block");
            };

- A block can also be declared, assigned, and invoked like a function (this includes taking arguments and returning values and capturing objects from the enclosing scope)

        void (^simpleBlock)(void) = ^{
                NSLog(@"This is a block");
            };

        simpleBlock();

- Use for common tasks such as collection enumeration, sorting and testing
### Use error objects for runtime problems
- Programming errors should be fixed before the app is shipped
- Runtime problems like running out of disk space or not being able to access a web service should be represented using NSError
### ObjC follows conventions
- method names use camel case starting with lower case, and are expressive

## Defining Classes
- Classes are blueprints for objects (data and action encapsulated)
- Interface specifies how an object is intended to be used i.e. the interface between instances of the class and the outside world (ecosystem or execution environment that the instantiated object lives in)
### Mutability
- Immutability and mutability can be represented, immutability - internal representation cannot be changed by other objects once instantiated
- NSString vs. NSMutableString - mutable contents can be changed at runtime
### Inheritance
- Defines the hierarchy of receiving behavior and properties from an object
- Root class provides base functionality
- Most non primitives in ObjC inherit from NSObject (the only other publicly known root object is NSProxy that doesn't inherit from NSObject but conforms to the NSObject protocol)
### Interactions
- Interface defines expected interactions, usually placed in separate header files
- basic syntax -

        @interface SimpleClass : NSObject

        @end

### Properties
- Properties are used for access

        @interface Person : NSObject

        @property NSString *firstName;
        @property NSString *lastName;

        @end

- * is to indicate that they are C pointers
- properties define their own accessibility

        @interface Person : NSObject

        @property (readonly) NSString *firstName;
        @property (readonly) NSString *lastName;

        @end

### Property Attributes
#### copy
- Creates an immutable copy of the object upon assignment and is typically used for creating an immutable version of a mutable object. Use this if you need the value of the object as it is at this moment, and you don't want that value to reflect any future changes made by other owners of the object.
#### assign
- Generates a setter which assigns the value directly to the instance variable, rather than copying or retaining it. This is typically used for creating properties for primitive types (float, int, BOOL, etc).
#### weak
- Variables that are weak can still point to objects but they do not become owners (or increase the retain count by 1). If the object's retain count drops to 0, the object will be deallocated from memory and the weak pointer will be set to nil. It's best practice to create all delegates and IBOutlet's as weak references since you do not own them.
#### unsafe_unretained
- An unsafe reference is similar to a weak reference in that it doesn't keep its related object alive, but it won’t be set to nil if the object is deallocated. This can lead to crashes due to accessing that deallocated object and therefore you should use weak unless the OS or class does not support it.
#### strong
- This is the default and is required when the attribute is a pointer to an object. The automatically generated setter will retain (i.e. increment the retain count of) the object and keep the object alive until released.
#### readonly
- This only generates a getter method so it won't allow the property to be changed via the setter method.
#### readwrite
- This is the default and generates both a setter and a getter for the property. Often times, a readonly property will be publicly defined and then a readwrite for the same property name will be privately redefined to allow mutation of the property value within that class only.
#### atomic
- This is the default and means that any access operation is guaranteed to be uninterrupted by another thread and is typically slower in performance to use.
#### nonatomic
- This is used to provide quicker (but thus interruptable) access operations.
#### getter=method
- Used to specify a different name for the property's getter method. This is typically done for boolean properties (e.g. getter=isFinished)
#### setter=method
- Used to specify a different name for the property's setter method. (e.g. setter=setProjectAsFinished)
### Method Declarations
- One object sends a message to another object by calling a method on that object
- basic syntax for declaration

        - (void)someMethod;

- (-) sign indicates instance method vs (+) for class methods
- (void) indicates the return type
- methods take parameters

        - (void)someMethodWithValue:(SomeType)value;

- multiple parameters

        - (void)someMethodWithFirstValue:(SomeType)value1 secondValue:(AnotherType)value2;

- to create unique class names, prefix the names in your application with three letters specific to your application
## Implementation
- written in source code file (.m or .mm)
- basic syntax

        # import "XYZ Person.h"

        @implementation XYZPerson

        @end

### Methods
- basic syntax for method implementation
        # import "XYZPerson.h"

        @implementation XYZPerson
        - (void)sayHello {
            NSLog(@"Hello, World!");
        }
        @end

### Public vs. Private Properties
- Public properties are declared in the header file
- Private properties are declared in an anonymous category or class extension in the implementation file

        # import "MyClass.h"

        // Class extension for private variables / properties
        @interface MyClass ()
        {
            // Instance variable
            int somePrivateInteger;
        }
        // Private properties
        @property (nonatomic, strong) NSString *firstName;
        @property (nonatomic, strong) NSString *lastName;
        @property (readwrite, nonatomic, strong) NSString *fullName;

        @end

        @implementation MyClass

        // Class implementation goes here

        @end

##@synthesize
- This call  automatically creates getter and setter methods for properties
## Major Note
- The LLVM compiler automatically synthesizes all properties so that there is no longer a need to explicitly write @synthesize
## Basic full implementation
- MyClass.h

        # import "SomeClass.h"

        // Used instead of # import to forward declare a class in property return types, etc.
        @class SomeOtherClass;

        // Place all global constants at the top
        extern NSString *const kRPErrorDomain;

        // Format: YourClassName : ClassThatYouAreInheritingFrom
        @interface MyClass : SomeClass

        // Public properties first
        @property (readonly, nonatomic, strong) SomeClass *someProperty;

        // Then class methods
        + (id)someClassMethod;

        // Then instance methods
        - (SomeOtherClass *)doWork;

        @end

- MyClass.m

        # import "MyClass.h"
        # import "SomeOtherClass.h"

        // Declare any constants at the top
        NSString *const kRPErrorDomain = @"com.myIncredibleApp.errors";
        static NSString *const kRPShortDateFormat = @"MM/dd/yyyy";

        // Class extensions for private variables / properties
        @interface MyClass ()
        {
            int somePrivateInt;
        }
        // Re-declare as a private read-write version of the public read-only property
        @property (readwrite, nonatomic, strong) SomeClass *someProperty;

        @end

        @implementation MyClass

        // Use # pragma mark - statements to logically organize your code
        # pragma mark - Class Methods

        + (id)someClassMethod
        {
            return [[MyClass alloc] init];
        }

        # pragma mark - Init & Dealloc methods

        - (id)init
        {
            if (self = [super init]) {
                // Initialize any properties or setup code here
            }

            return self;
        }

        // Dealloc method should always follow init method
        - (void)dealloc
        {
            // Remove any observers or free any necessary cache, etc.

            [super dealloc];
        }

        # pragma mark - Instance Methods

        - (SomeOtherClass *)doWork
        {
            // Implement this
        }

        @end
## Delegation
- Common pattern in Mac dev
- One object acts on behalf of or in coordination with another object
- Delegating object stores a reference to the delegate and sends messages when necessary
- Delegate is informed that the delegating object has just handled an event on behalf of it
- Delegate responds by updating its appearance or the appearance of other objects
- Advantage: easily customize behavior of several objects through one main object (similar to pub/sub (observer), adapter, mediator, and proxy)
- In managed memory, the delegating object maintains a weak reference to its delegate, in garbage collected environment the receiver maintains a strong reference to its delegating object
- Example: NSWindow - the actual window object sends messages on events, i.e. the window close button, when clicked, sends a message to the windowShouldClose method inside the windowDelegate to implement the correct behavior which returns a boolean object depending on the success of the result
- A delegate is generally automatically registered as an observer of notifications posted by the delegating object.
- Observers receive notifications, delegates receive messages; delegating objects send notifications, delegates send messages
## Objects
### Messaging
- Messaging is basically just method calling

        [someObject doSomething];

- Object reference on the left is sent the doSomething message which is essentially like calling C function
### Pointers
- regular variables are declared as expected, local variables declared the same way, but specifically in the scope of a method definition, and this automatically allows for memory management of these variables, i.e. alive for the calling period of the lifetime of the method
- Generally an object in Objective-C needs to stay alive longer than the variable that it was assigned to, which leads to dynamic memory allocation (was controlled using garbage collection, now ARC or automatic reference counting)
- This system leads to the use of pointers for object instantiation within a scope
### General
- Objects can be passed as method parameters
- Methods can return values
- Objects can send messages to themselves...this is done using self

        @implementation XYZPerson
        - (void)sayHello {
            [self saySomething:@"Hello, world!"];
        }
        - (void)saySomething:(NSString *)greeting {
            NSLog(@"%@", greeting);
        }
        @end

- Objects can call methods implemented by their own superclasses
- Objects are created dynamically
- NSObject root class defines the alloc and init methods that respectively allocate and instantiate the object

        NSObject *newObject = [[NSObject alloc] init];

- Initializer methods can take arguments...NSNumber

        - (id)initWithBool:(BOOL)value;
        - (id)initWithFloat:(float)value;
        - (id)initWithInt:(int)value;
        - (id)initWithLong:(long)value;

- Class factory methods can be called to both alloc and init an object

        + (NSNumber *)numberWithBool:(BOOL)value;
        + (NSNumber *)numberWithFloat:(float)value;
        + (NSNumber *)numberWithInt:(int)value;
        + (NSNumber *)numberWithLong:(long)value;

- Use new to alloc and init with no arguments
- Some objects can be created with literals

        NSString *someString = @"Hello, World!";
        NSNumber *myBOOL = @YES;
        NSNumber *myFloat = @3.14f;
        NSNumber *myInt = @42;
        NSNumber *myLong = @42L;
        NSNumber *myInt = @(84 / 2);

### Dynamic Language Attributes
- Since pointers are used, Objective C allows for dynamic typing but always calls the correct method depending on the type stored
- The id type is generic and can be used to instantiate an object, but you lose some compile time information in this case

        id someObject = @"Hello, World!";
        [someObject removeAllObjects];

### Equality
- use == to test for equality
- since objects are referenced using pointers, == when used with objects determines if the pointers are pointing to the same object
- To test if the objects represent the same data use the isEqual: method
### Basic Initialization
- Always initialize variables to avoid garbage memory in the location of the variable
- For objects, the compiler automatically sets object pointers to nil
- Checking objects

-

        if (somePerson != nil) {
            // somePerson points to an object
        }

        if (somePerson) {
            // somePerson points to an object
        }

        if (somePerson == nil) {
            // somePerson does not point to an object
        }

        if (!somePerson) {
            // somePerson does not point to an object
        }

# Ivars vs. Properties
- If an ivar is only going to be assigned to once during the lifetime of the object, you don't really gain anything by declaring a property. Just retain/copy/assign during init and then release as necessary during dealloc.
- If an ivar is going to be changed frequently, declaring a property and always using the accessors will make it easier to avoid memory management errors.
- You can declare properties in a class extension in the .m file rather than the .h file if the properties and ivars are meant to be private.
- When targeting iOS 4.0+, you don't need to declare ivars at all in your header if you define a property and synthesize accessors.

# Modern Objective C
## instancetype
- Use the instancetype keyword as the return type of methods that return
an instance of the class that they are called on
- alloc
- init
- factory methods
- Use instancetype as the return type over id to promote type safety
- Replace id for return values,  especially for methods other than alloc
init or new because the compiler does not automatically convert these
## Properties
- Properties capture the state of the object and can be public or private
- Properties provide these benefits over instance variables:
    - Autosynthesized getters and setters
    - The naming conventions of auto generated getters and setters is more clear
    - Properties provide keywords that make attributes of the property more clear
    i.e. assign, copy, weak, atomic, nonatomic
- Naming conventions - getter method is name of the property, setter is setName
i.e. set + (Capitalized name of property)
- Boolean properties have a getter starting with is, i.e. is + (Capitalized name of property)
- Follow these rules:
    - init, copy methods, factory methods, methods that perform actions and return bools,
    and methods that explicitly change internal state are not properties
    - a read/write property has two accessors methods, a getter and a setter
    use the readwrite keyword for these
    - a read-only has a getter, use readonly keyword
    - getter s/b idempotent, i.e. returns the same value no matter how many times
    it is called. The getter can compute the value each time it is called though
    (but should be considered for efficiency in certain cases)

# Enumeration Macros
- Use NS_ENUM macro to define a set of values that are mutually exclusive, with a type of NSInteger
- Use NS_OPTIONS to define a set of bitmasked values that can be combined together
- NS_OPTIONS defines a name and a type but should be used with NSUInteger
- replace plane enums with NS_ENUM and enums used for bitmasks with NS_OPTIONS

# Object Initialization
- Init methods are used to call the superclass constructor (init method) and then
initialize the object's own methods. Any other initializer is known as a convenience
initializer, which usually delegate to a higher initializer, ending in the default.
- The work of init method is to make sure that all instance variables are properly
initialized
- Can use the macro NS_DESIGNATED_INITIALIZER
- If multiple initializers are created, the class must implement all of the
designated initializers of its superclass

# ARC
- Avoids explicit use of retain, release, and autorelease required by garbage collection
- In Xcode, use Edit > Refactor > Convert to Objective-C ARC

# Notes from Book
## Ch. 3
- Objective-C built on C but takes object orientation influence from smalltalk
- Started with a tie to GNU C, but has moved over to clang/LLVM
## Create and Use Instances
- Call methods (pass messages) like [Object method]; or Object.method;
- [Object alloc] - returns a pointer to the object's memory space
- init makes sure the object is fully initialized, which is required before use
- Use like this:
        NSMutableArray *foo;
        foo = [NSMutableArray alloc];
        [foo init];
    or:
        NSMutableArray *foo;
        foo = [[NSMutableArray alloc] init];
- Methods with arguments:
        [object methodOne:arg];

    or:
        [object methodOne:arg1
                methodTwo:arg2
                methodThree:arg3]
- Use @ before strings to differentiate between C strings and NSString
- Messaging (method calling) on nil is ok in Objective C
- NSArray is created with the objects that will be in it for its lifetime (i.e.
it is immutable), NSMutable array provides the ability to add or remove objects
- Use NSString for strings
- Do not subclass NSString or NSArray
- Use description methods to define how an object is described by NSLog when logged
with %@, i.e. return value is what is logged
- NSDate instances represent a single point in time and are essentially immutable
## Writing Initializers
- Call superclass initializer
- init own instance variables
- return self, a pointer to the object itself
- also common to assign the return value of the superclass init to self and then
test that self has been instantiated with if(self) because some classes
return nil if the initialization was not possible
- can define additional initializers to pass arguments to the initializer
that are values to use for instance variable instantiation, i.e. initWithInt, etc
- can also override init methods to use init with argument methods
### Follow these rules
- don't create an init method if the superclasses is sufficient
- otherwise, you must override the superclasses designated initializer
- multiple initializers delegate to the designated initializer
- designated initializer must call superclass init

# Ch4 Memory Mgmt
- Three approaches:
    - retain counts - the most basic, any object that has a pointer to another
    object increments the retain count for that object
    - garbage collection - watches the object graph and deallocates memory for
    unreachable objects
    - ARC - uses retain counts to allow the compiler to automatically manage
    memory for you
- Manual reference counting requires work on the programmers part which will
result in crashes if a retain cycle is created
- Garabage collection can impact performance. Garbage collection does not work
for OSX prior to 10.5 and is not portable to iOS
- ARC is a compromise for a middle ground but does not solve all problems.
It still requires manually allocated memory to be manually deallocated and can
result in retain cycles.
## Manual Reference Counting
- alloc sets retain count to 1
- when retain count is 0, the object is deallocated
- can increment manually using the object's retain method (i.e. messaging retain)
- decrement by sending the release method
- the retain count should indicate how many other objects possess a reference
to the object
### Rules for retain and release
- implement a dealloc method for how to manage memory deallocation for an object
- when an object with a retain count of 1 is messaged release, the dealloc method
is called
- Methods that create objects internally create retain counts for those objects,
which can result in memory leaks. Explicitly deallocate these objects when
appropriate
- sometimes it is not possible to release the internal objects in methods because
you need to return the object from the method.
- Use autorelease pools in those cases. Send the object the message autorelease
which marks it to be sent release some time in the future
- the objects are released when the pool is drained. Pools are created and drained
before and after every event is handled.
- For non GUI applications, like CLI programs, there is no event loop and so an
autorelease pool has to be created explicitly.
- These pools can be nested for memory management
- Rules:
    - alloc, new, or copy increment the retain count to 1 and creates ownership
    - objects created through other means are set for autorelease
    - to keep these objects past autorelease cycles, send it the retain message
    - to remove an object, send it release or autorelease
    - when an object has at least one owner it will continue to exist until
    its retain count goes to 0 and it is sent the dealloc message
## ARC
### Strong references
- With ARC, strong references are:
    - default
    - retained implicitly
    - when changed, old object is released and new is retained
    - ARC manages dealloc
    - can still implement dealloc for other cleanup
- weak references are:
    - similar to manual reference counting
    - no implicit retain
    - may cause crashes when a pointer to an object is not retained, object is deallocated,
    and pointer points to bad memory
- use a strong reference with a parent to child relationship and a weak reference
for a child to parent relationship
- ARC works with manual reference counted code and on a per file basis
- Xcode has a migration tool under Edit menu
- weak references are not supported on older platforms
- Property names cannot begin with new
- With ARC, do not call retain, release, autorelease or dealloc

# Ch5 Target/Action
- NSControl is the parent class for many UI objects in AppKit
- A control has a target and an action
    - target is a reference to another object (pointer)
    - action is a message to send to the target (method or selector to call)
- NSObject -> NSResponder -> NSView -> NSControl
- Action method takes the sender as an argument and can also have a callback
that can be used to query the sender for more information
## NSObject
- the root object for most Objective-C classes
- Interface:
    - initialize
    - load
    - alloc
    - allocWithZone
    - init
    - copy
    - copyWithZone
    - mutableCopy
    - mutableCopyWithZone
    - dealloc
    - new
    - class
    - superclass
    - isSubClassOfClass
    - instancesRespondToSelector
    - conformsToProtocol
    - methodForSelector
    - instanceMethodForSelector
    - description
    ...
## NSResponder
- abstract class that forms the basis of event and command processing in AppKit
- any class that handles events must inherit from NSResponder
- three components:
    - event messages
    - action messages
    - the responder chain
- plays an important part in presenting error information
- first responder - an object is the first object to receive key event and action
message
- Methods:
    - acceptsFirstResponder ...
    - nextResponder
    - Events:
        - mouseDown
        - mouseDragged
        - mouseUp
        - mouseMoved
        - mouseEntered
        - mouseExited
        - rightMouseDown
        ...
        - keyDown
        - keyUp
        - performKeyEquivalent
        ...
        - cursorUpdate
        - tabletPoint
        - scrollWheel
## NSView
- defines basic drawing, event handling, and printing architecture of an app
- generally should not use this
- subclassing
    - create custom subclasses of NSView to draw UI objects and respond to UI
    events in special ways
    - do not call super if your view implements many of the common event handlers
## NSControl
- fundamental features
    - drawing to screen
    - responding to user events
    - sending action messages
- closely linked to NSCell
- does not have a delegate specifically, but provides delegate methods
- Methods:
    - initWithFrame - specifies frame (rectangle) for view object initialization
## NSWindow
- object to manage and coordinate the windows that an application displays on the screen
- allows window collections and provides methods to manage these
- Methods:
    - initWithContentRect
    - styleMask
    - setStyleMask
    - toggleFullScreen
    - worksWhenModal
    - alphaValue
    - backgroundColor
    - contentView
    ...
## Common NSControl Subclasses
- NSButton
- NSSlider
- NSTextField
## Set action target programmatically
- use NSControl's setAction which takes a selector
- use @selector(targetMethod:)
# Ch6 Helper Objects
- Use helper objects to avoid subclassing and extending objects
- Cocoa objects often have an instance variable called delegate that allows
you to set the helper object
- When you instantiate an instance variable of a type of object within an object,
you set the containing object to be the delegate of the instance variable
- When the instance variable performs actions that trigger methods on the delegate,
the implemented delegation methods will be called
- In the speakline example, appdelegate is the also the delegate for speechSynth
and when the speechSynth finishes speaking, it calls the didFinishSpeaking method
on the AppDelegate
- Full process:
    - Make sure delegate object inherits from the delegate superclass inside
    implementation
    - Set delegate outlet to object in UI if necessary
    - Call setDelegate in init function or where appropriate
    - Generally the relationship will be that the delegator object will be
    an instance variable/property of the delegate and the init method will
    construct the ivar and then call setDelegate:self
    - implement the desired methods of the delegate superclass
## dataSource
- different view objects like NSTableView have a dataSource
    - means must implement numberOfRowsInTableView which receives a response
    from the dataSource about the number of rows
    - and tableView:objectValueForTableColumn:row: which receives a response
    from the dataSource with the values
- for the NSTableView - set the content type to cell based and, make the view
have one column and disable column selection
- set delegate of the table view, and create an outlet called tableView in header
and connect it
# Ch7 Key-Value Coding and Observing
- Allows setting and getting of property values by name i.e.
        [obj setValue:val forKey:@"keyName"];
        [obj valueForKey:@"keyName"];
- Provides hooks into an object to observe changes to properties
- View objects provide the ability to bind their output values to keys
- When an object is bound like this, it also receives notifications for when
the key is changed by its getters or setters
- to ensure that the UI gets updated when changing a key use:
    - self willChangeValueForKey and didChangeValueForKey
    - self setValue: forKey:
    - or self setKeyName:
- Objective-C also provide dot notation for property access
- behind the scenes, the language uses key paths
    - obj valueForKeyPath:@"object.at.key.path"
- this can be used to create bindings programmatically (i.e. between UI objects
and code objects)
- To programmatically observe key value changes:
    - [Delegate addObserver:self forKeyPath:@"keyName" options:NSKeyValueChangeOldKey context:somePointer];
- This method is defined in NSObject
# Ch 8 NSArrayController
- NSController was introduced to abstract the controller in MVC for Cocoa
- NSArrayController has an array of data objects that it serves as a controller
for
## Document Based Application
- application in which several documents can be opened simultaneously
- in RaiseMan, Person class is a model class
- To set up an array controller, drop the array controller into the dock in
the document view nib. Set the object controller section to the class and keys
that will be used in the array controller and bind the array controller to the
file's owner, setting model key path to the property that it will map to.

- Never bind a scroll view or a cell view when working with a table view

- Bind the columns of the table view to the properties in the array controller
as arrangedObjects
- Use number formatters to display number formats
- Buttons can be linked to keyboard entries through the attribute inspector
- This example works through key-value coding. The array controller coerces
values from the bound source to the type it expects (through the number formatter, etc).
- Pointers can be nil but floats cannot
- When a value comes in that is nil, the setNilValueForKey can set a default value
- columns allow sorting criteria to be set in the attributes inspector
# Ch 9 NSUndoManager
- Use NSUndoManager to keep track of changes needed to manage the undo stack
- Use NSInvocation to package messages as objects
- this can be used for message forwarding
- the undo manager has an undo stack and a redo stack, and methods need to have inverse methods
for managing how to redo (some methods can be their own inverse)
- Each instance of NSDocument has an undo manager
## Key-value coding and relationships
- simple attributes - strings, dates, data, numbers
- to-one relationship - objects that have a relationship to complex objects maintain a pointer to
those objects
- ordered to many - an object that is an ordered list has many of a type and
is usually an instance of NSMutableArray
- unordered to many - a grouping of any objects that have no particular order but form
the basis for a larger object

- to avoid circular dependencies with header/implementation files
    - you can use # ifndef macros or...
    - import the header of one class into the implementation of another and use
    an @class forward definition in the header file of that class
## Key-value observing
- use NSObjects addObserver method to observe changes on a key. THe method
takes a key path to observer the key.
- options defines what you want when notified of changes and context defines
additional data that should be sent
- the observing object must implement observerValueForKeyPath
- it is possible to intercept messages intended for other classes, this is
what the context param is good for. Use a class specific pointer value as the
context argument
# Ch 10 Archiving
- An object oriented program constructs a complex object graph representing
its internal structure while it is running
- Archiving (or serialization) is the process of converting data like this
graph into a stream of bytes
- Recreation from the stream is unarchiving
- This is the process used with NIB files when starting an application
- Only object names and instance variables (data) go into the NIB
- Two steps: define how an object archives, then trigger archiving
- Protocol: when a class promises to implement all of the methods in a pre-defined
interface
## NSCoder and NSCoding
- NSCoder is an abstraction for a stream of bytes
- initWithCoder reads data from a stream and saves that to an instance variable
- encodeWithCoder reads those variables and writes them to a byte stream
- NSCoder is an abstract class, only create instances of subclasses of NSCoder
- NSCoder has methods to encode different types (e.g. encodeBool, encodeInt)
- most objects implement NSCoder except NSObject (if parent does, include super:encodeWithCoder
in encodeWithCoderImplementation)
- to decode, add initWithCoder and user decodeObjectForKey, decodeFloatForKey, etc
## Document Architecture
- follows MVC, Document.h/.m act as controller
- does the following:
    - save model data to a file
    - load model data from a file
    - display model data in views
    - take user input from views and update the model
## Info.plist
- Info.plist contains information like what type of files the application works with
- Document based apps create an instance of NSDocumentController, which is rarely used
directly
- handles save requests, etc
- can access through:
        NSDocumentController *dc;
        dc = [NSDocumentController sharedDocumentController];
## NSDocument
- Save, save as, and close use NSDocument to save data to a file.
- For this to work, you must implement one of these three:
    - dataOfType - saves file as NSData object - buffer of bytes
    - fileWrapperOfType - saves as NSFileWrapper
    - writeToURL - document is responsible for how to encode/write file to
    URL (given a document type)
- Opening requires reading the saved date and one of these three:
    - readFromData
    - readFromFileWrapper
    - readFromURL
- A document also creates an instance of NSWindowController for each open window
- Need to subclass NSWindowController for:
    - multiple windows open in same document (i.e. toolbars, etc)
    - need to separate UI controller and model controller logic
    - create a window without a corresponding NSDocument object
## NSKeyedArchiver and Unarchiver
- Use NSKeyedArchiver which uses the class method archivedDataWithRootObject to archive into and
NSData buffer of bytes and save a document
    - this requires implementing the document's auto generated dataOfType method
- Use NSKeyedUnarchiver which uses the class method unarchiveObjectWithData to open a document
    - this requires implementing the auto generated readFromData method
## Extension and Icon for File
- the extension was configured when the project was started
- add an .icns file and in the info tabe under Document Types and set settings
## Applications
- applications are directories
- directory contains Info.plist for app startup info (also used by finder)
- MacOS/ contains executable code
- Resources/ which contains images, sounds, NIB files, etc
## Preventing Circular Encoding
- Objects that have circular references in the object graph could cause
encoding cycles
- To avoid this, each object is given a unique token when encoded which is added
to a table of encoded objects. When the object is supposed to be encoded again
it adds a token to the encoding stream instead of encoding it again
## Creating a protocol
- Create .h file
        @protocol Protocol
        - (void)method1
        - (void)method2
        @optional
        - (void)method3
        @end
- Other class implementation - import protocol
        @interface Interface : NSObject <Protocol, OtherProtocol>
## Autosave
- add autoSavesInPlace
## UTIs
- mac solution to defining data types through type identifier strings
# Ch 11 Basic Core Data
- Simplifies the MVC, document based, save to file pattern that is common in
apps. Uses NSArrayController, bindings, and NSManagedObjectContext.
- the application reads info about properties and types of data for saving
from .xcdatamodeld which contains an object model
- uses entity and property, with attributes being simple data and relationships
being more complex data.
- View based table views came around after 10.7 and iOS and allow customization
of the table view
- when you bind a multi column table view to an array controller, you do it
on the parent table and bind to the array controller's arrangedObjects. You then
bind each of the child column elements to the parent objectValue.property
- Core data creates a number of objects automatically:
    - NSPersistentDocument
    - NSManagedObjectModel
    - NSEntitiyDescription
    - NSAttributeDescription
    - NSManagedObjectContext * - generally most used in core data
- To modify the default behaior for anything done automatically, subclass
NSArrayController and override the newObject method
# Ch12 NIB Files and Window Controllers
- NSWindow controller loads nib files which needs to be done when different
windows show up, saves memory to not load it instantly
- Use NSPanel to add a preferences panel to an application (subclass of NSWindow)

