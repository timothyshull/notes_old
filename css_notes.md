# Media Queries
- Used to limit the style sheets' scope depending on the current media type
- can combine logical operators not, and, and only to limit the scope of the rules
- comma separated lists function as logical or in the context of media queries
### and
- combine queries
- combine media features with media types
### comma separated lists
- or
### not
- returns true if the media query would otherwise return false
## media types
- all
- aural
- braille
- handheld
- print
- projection
- screen
- tty
- tv
- embossed
## media features
- width
- min-width
- max-width
- height
- min-height
- max-height
- device-width
- min-device-width
- max-device-width
- device-height
- min-device-height
- max-device-height
- aspect-ratio - ratio of horizontal pixels to vertical pixels
- min-aspect-ratio
- max-aspect-ratio
- device-aspect-ratio
- min-device-aspect-ratio
- max-device-aspect-ratio
- color - # of bits per color for device
- min-color
- max-color
- color-index
- min-color-index
- max-color-index
- monochrome - bits per pixel on a monochrome (greyscale) device
- min-monochrome
- max-monochrome
- resolution - pixel density of the output device
- min-resolution
- max-resolution
- scan - describes the scanning process of television output devices
- grid - determines if the device is a grid or a bitmap device
- orientation - landscape or portrait

# CSS Counters
- Used as variables to keep track of the number of times they are used
- Can nest counters, and include counters as content

        body {
            counter-reset: section;
        }
        h3::before {
            counter-increment: section;
            content: "Section" counter(section) ": ";
        }

# CSS Gradients
- Create smooth transitions between two or more colors without using images
- linear gradient and radial gradient

# CSS Transforms
- Similar to svg transforms, modify coordinate space of a CSS element without disrupting normal
document flow

# CSS Animations
- animate transitions from one set of CSS styling rules to another
- Advantages:
    - easy to use for simple transformations
    - run smoothly even under load
    - browser runs it so it can optimize
- Properties:
    - animation-delay
    - animation-direction
    - animation-duration
    - animation-iteration-count
    - animation-name
    - animation-play-state
    - animation-timing-function
    - animation-fill-mode
### Using Keyframes
- First configure timing then set the appearance using @keyframe to set how the animation is
rendered at specific points in time
- Use a percentage to set the point in the animation that the keyframe should appear

# CSS Transitions
- Allows changes in a property to take place over time
- Refer to the list of animatable properties
- Properties:
    - transition-property - the CSS property that should be transitioned
    - transition-duration - the time period for the transition
    - transition-timing-function:
        - ease
        - linear
        - step-end
        - steps(4, end)
    - transition-delay
- A CSS transition event triggers a JavaScript transitioned event on transition end

# CSS Multiple Backgrounds
- It is possible to apply multiple backgrounds that are layered

# CSS Flex Boxes
- Used for a specific layout mode to enable predictable behavior across multiple screen
sizes and device types
- Provides an improvement over the block model, avoiding floats, and prohibiting margin collapse
- Direction agnostic
    - block is vertically-biased
    - inline is horizontally-biased
- Benefit is in layouts that have to change orientation, resize, or stretch to accomodate
devices
- Good for small layouts where the grid layout is better for large layouts
- Flex container - parent element, uses flex or inline-flex display property
- Flex item - each child of a flex parent
- Axes - main axis is the axis along which the flex items follow each other, cross axis is
perpendicular to the main axis
    - flex-direction - establishes the main axis
    - justify-content - establishes how flex items are laid out along main axis
    - align-items - defines flex item relationship to cross axis
    - align-self - defines how a single flex item is related to the cross axis, overriding
    the align-items property
    - order - assigns elements to an ordinal group and determines which elements appear first
    - flex-flow - shorthands flex-directiona dn flex-wrap

# CSS Multiple Columns in Blocks
- column-count
- column-width
- columns
- height or max-height
- column-gap
- for non-column supporting browsers, use vendor prefixes -moz and -webkit

# Concepts
## Cascade
- The three main sources of the cascade are
    - the browser's default styling
    - User specified styles
    - Styles specified by the document's author in:
        - an external file
        - in a style tag at the beginning of the document
        - in a style attribute on an element in the document
- Priority: author, user, browser
- When a child element inherits styles from its parent, the child's defined styles have
priority over the parent's
### W3C Cascade Definitions
- Specified values are
    - use the resulting cascade value if it exists
    - use the inherited property otherwise
    - otherwise use the property's initial value indicated in the property definition
- Computed values
    - specified values are resolved to computed values - URI's becoming absolute and em and ex
    units are computed to absolute values
- Inherited values
    - are specified in the definition or in the author stylesheet using the inherit rule
- The @import rule
    - allows users to import rules from other stylesheets
        - can be @import "mystyle.css"; or @import url("mystyle.css");
- The cascade:
    - find all declarations that apply to the element and property in question using matching
    selectors and @media rules
    - sort by importance
        - user agent declarations
        - normal declarations
        - author normal declarations
        - author important declarations
        - user important declarations
    - sort rules with the same importance and origin by specificity; higher specificity
    overrides lower specificity
    - sort by order specified, i.e. placement in stylesheet etc. Imported stylesheets
    are considered to have been declared before the stylesheet that is currently importing it
- Important rules are used to override precedence
- Specificity can be calculated using the rules detailed in W3C spec
## Syntax
- composed of property and value pairs separated by a colon - color: red
- grouped into block declarations - semi-colon terminated property-value pairs, with last one
not necessarily semi-colon terminated, wrapped in curly braces
- valid declarations are preceded by a selector(s) to scope the rulesets
- statements can be rulesets or at-rules, @media rules can be nested
## Box Model
- each element is represented as a rectangular box
- these boxes are defined using the standard box model with four edges: margin edge, border
edge, padding edge, and content edge
- content area - includes background, image or text of element
- padding area - space between the content and the border
- border area - space between border and margin
- margin area - space between border and element's neighbors
- margin collapsing happens when margins are not clearly defined because elements share margins
- content-box - default - width and height are calculated using only the content and exclude padding
border and margin
- padding-box - width and height measurements include content and padding, but exclude border and margin
- border-box - width and height measurements include content, padding and border but exclude marginA

# Selectors
## Type selectors
- selects elements by node name (i.e. html element node name) and applies to all elements of that type
in the document
## Class selectors
- match based on the contents of the element's class attribute (.classname is equivalent to
[class~=classname])
## ID selectors
- matches based on the contents of an element's id attribute (#id_value or [id=id_value]). In the
cascade, ids have a higher specificity than classes
## Universal selectors
- * is the universal selector, it matches a single element of any type
- it can be used with namespaces in CSS3
    - ns|*, *|*, |*
## Attribute selectors
- attribute selectors match elements based on the presence of a given attribute or attribute value
    - [attr] - matches elements that have the attribute named attr
    - [attr=value] - matches elements with attribute attr and value exactly value
    - [attr~=value] - value can be exactly one of a whitespace-separated list of words
    - [attr|=value] -
