# 1. Define Information Architecture (IA)
1. Determine content
2. Create page structure
3. Add content

# 2. Make it Responsive
1. Add a viewport

        <meta name="viewport" content="width=device-width, initial-scale=1">

2. Add simple styling - start with a style guide, use images to draw attention to content (background images, use background-size: cover)
3. Use media queries to set breakpoints where the content starts to look bad
4. Constrain the maximum width of the design

        .container {
              margin: auto;
              max-width: 800px;
            }

5. Alter the padding and reduce text size

        #headline {
              padding: 20px 5%;
            }

6. Adapt elements to wide viewport - can move from stacked elements to a new IA
7. Float elements to allow transition from inline to stacked
8. Tile images
9. Make images DPI responsive
10. Tables - transition to one column for narrow viewports using media queries

### General guidelines
1. Always create a basic IA and know content before coding
2. Always set the viewport
3. Create base experience around mobile first
4. Once mobile experience is set, increase viewport and set breakpoints accordingly
5. Keep iterating

## Home Page and Navigation Principles
1. Use desktop/mobile links for choice vs full/mobile
2. Put calls to action (main purpose of page) centered both vertical and horizontal on mobile
3. 
## Images in Markup
1. Use max-width over width and relative sizes (ie %), provide alts
2. Use srcset to select images based on the device

        <img src="photo.png" srcset="photo@2x.png 2x" ...>

3. Use picture to provide multiple versions of an image, use picture polyfill to avoid browser support issues

        <picture>
            <source media="(min-width: 800px)" srcset="head.jpg, head-2x.jpg 2x">
            <source media="(min-width: 450px)" srcset="head-small.jpg, head-small-2x.jpg 2x">
            <img src="head-fb.jpg" srcset="head-fb-2x.jpg 2x" >
        </picture>

4. Can provide width descriptor along with the size of the image elements to relatively size

        <img src="lighthouse-200.jpg" sizes="50vw"
                 srcset="lighthouse-100.jpg 100w, lighthouse-200.jpg 200w,
                         lighthouse-400.jpg 400w, lighthouse-800.jpg 800w,
                         lighthouse-1000.jpg 1000w, lighthouse-1400.jpg 1400w,
                         lighthouse-1800.jpg 1800w">

5. Specify percents depending on view width

        <img src="400.png"
                sizes="(min-width: 600px) 25vw, (min-width: 500px) 50vw, 100vw"
                srcset="100.png 100w, 200.png 200w, 400.png 400w,
                        800.png 800w, 1600.png 1600w, 2000.png 2000w">

6. Make product images expandable
7. Try the compressive image technique to reduce size, but use caution
8. JavaScript image replacement (window.devicePixelRatio, navigator.connection)

## Images in CSS
1. Use media queries for conditional image loading

        @media (min-width: 500px) {
              body {
                background-image: url(body.png);
              } 

2. Use image-set to provide high res images 
3. media queries can use device pixel ratio (dppx) to use different images depending on resolution
4. Use media queries to conditionally download images

## Use SVG for Icons
1. Replace simple icons with unicode (&#9733)
2. Replace complex icons with SVG
3. Use icon fonts with caution (limited styling, pixel perfect position is a challenge, not semantic, can result in large file size)

## Optimize Images for Performance
1. Choose the right format 
    - Use JPG for photographic images.
    - Use SVG for vector art and solid color graphics such as logos and line art. If vector art is unavailable, try WebP or PNG.
    - Use PNG rather than GIF as it allows for more colors and offers better compression ratios.
    - For longer animations, consider using <video> which provide better image quality and gives the user control over playback.
2. Reduce file size
3. Use image sprites with positioning to reduce number of images
4. Consider lazy loading - infinite scrolling experiences or reduce page load times

## Avoid Images Completely
1. Place text in markup instead of embedded images
2. Use CSS to replace images

## Image Optimization
1. Consider replacement - CSS, webfonts
2. Vector vs. raster