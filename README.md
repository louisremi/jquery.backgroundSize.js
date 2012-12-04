jquery.backgroundSize.js **DEPRECATED**
========================

**This plugin has been deprecated**, please use its [background-size polyfill](https://github.com/louisremi/background-size-polyfill) successor.

A jQuery cssHook adding support for "cover" and "contain" to IE6-7-8, in 1.5K

Introduction
------------

[Demo](http://louisremi.github.com/jquery.backgroundSize.js/demo/)

**Progressive Enhancement** is the mantra I live by. It means *"Have fun with CSS3 and don't worry about IE6-7-8 users; they'll never notice they're missing out on your gorgeous text-shadows and gradients, anyway"*.

All was well until I discovered the elegance of `background-size: cover;` and `background-size: contain;`.
The first one, for instance, allows an image to completely cover a background, 
**without** having to send a 1920x1080 background image down the pipes.

Unfortunately, they don't degrade gracefully: websites would likely appear broken to IE6-7-8 users :-( 
...unless you use this cssHook!

How does it work?
-----------------

Set the `background-size` just like any other style property with jQuery:

```javascript
$(elem).css( "background-size", "cover" );
```

In browsers that don't implement it natively, an `<img/>` will be inserted in the background of `elem` and emulate the `cover` or `contain` value.

Limitations
-----------

Calculating the displayed position and size of the background-image of an element is quite complex and function of numerous parameters:  
- the size of the element itself  
- the size of the image  
- the values of background-[size/position/clip/origin/attachment/scroll]

It is thus impossible to emulate `background-size` completely and perfectly. But it's still possible to enjoy the main features:  
- correct position and size of the background image  
- updated position and size on browser resize  
- updated image, position and size when the background-image is modified using `$(elem).css("background-image", "differentImage.jpg");`

The following style properties, values or behavior aren't supported:  
- values other than `cover` or `contain` in `background-size`  
- multiple backgrounds (although the :after trick can still be used)  
- 4 values syntax of `background-position`  
- lengths (px, em, etc.) in `background-position` (only percentages and keywords such as `center` work)  
- any `repeat` value in `background-repeat`  
- non-default values of background-[clip/origin/attachment/scroll]  
- updated image size when the size of the element is modified

Removing any of these limitations is probably just one fork away...

Development vs. Production
--------------------------

The elements targeted by this plugin should be positionned (`position: relative/absolute/fixed;` but not `static`) and have a z-index. 
If not, they will be given a `position: relative;` and `z-index: 0;`.

In some cases this could potentially alter the layout of your page in IE6-7-8.
To help you spot problems earlier, you can use a flag that will turn ON emulation of `background-size` in all browsers:

```html
<!-- The flag should be inserted before loading the script -->
<script>$.debugBGS = true;</script>
<script src="/path/to/jquery.backgroundSize.js"></script>
```

Switch to the following snippet before deploying your code to a production environment:

```html
<!--[if lt IE 9]> <script src="/path/to/jquery.backgroundSize.min.js"></script> <![endif]-->
```

Refreshing a background
-----------------------

There are several cases that can cause an element to be resized and require its background to be refreshed
(e.g. modifying the size of its content or animating it).
This plugin includes a helper for that purpose: `$.refreshBackgroundDimensions( elem );`. Here's how to use it during an animation:

```javascript
$(elem).animate({ height: "+=100" }, {
	step: function() { $.refreshBackgroundDimensions( this ); }
});
```

License
-----------------

MIT Licensed http://louisremi.mit-license.org/, by [@louis_remi](http://twitter.com/louis_remi)