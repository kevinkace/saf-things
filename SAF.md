# Making a Spicy Release Page for Super Adventure Festival

The yearly [Super Adventure Festival](https://wiki.guildwars2.com/wiki/Super_Adventure_Festival) originated as a one-time [release](https://www.guildwars2.com/en/the-game/releases) for [April Fools' day 2013](https://www.guildwars2.com/en/the-game/releases/april-2013/). From the start it was hugely popular with many players, (and not so popular with others, but let's ignore them for now).

We've always created a release page for each of the 50+ releases that have been part of [Guild Wars 2](https://www.guildwars2.com/en) since GW2 released in Fall 2012. This year, for the 55th release we had our shit especially together and were able to add some fun *fun* **FUN** features to the release page, reminisent of the 80's & 90's, of which Super Adventure Festival harkens back to.

Everything at [ArenaNet](https://www.arena.net) is a team effort, and this release page was a combined effort of the marketing, web, and art departments. Three weeks before SAF began, we locked ourselves in a small meeting room and came up with a list of features we thought would amuse our fans, and that we could complete in the short amount of time we had (amongst all our other commitments).

### Note: the code here has been truncated for clarity

## Gifs

A requirement for... well, just about *everything*. All the things should have gifs. Photoshop has 2 methods for creating animated GIFs: video timeline, and frame animation. Here's [a tutorial from Adobe](https://helpx.adobe.com/photoshop/how-to/make-animated-gif.html) showing one method.

## Marquee

Perhaps one of the most neglected staples of the early web; one that's never really had a retro resurgence. Until today! The [`<marquee>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/marquee) element has been obsolete for some time. And even though most browsers still support it, re-implementing the feature with modern web technologies is more fun.

```html
<div class="marquee">
    <p>Marquee text.... * More marquee text</p>
</div>
```

```css
.marquee {
    overflow: hidden;
    position: relative;
    width: 940px;
    height: 2em;

    p {
        position: absolute;
        white-space: nowrap;
        transform: translateX(940px);

        animation: 40s marquee linear infinite;
    }
}

@keyframes marquee {
    100% {
        transform: translateX(-100%);
    }
}

```

[Working example & code](http://codepen.io/kevinkace/pen/vxrqJj?editors=1100)

## Custom Cursor

Those who are familar with Super Adventure Box might recognize [Helping Hands](https://wiki.guildwars2.com/wiki/Helping_Hand_(Super_Adventure_Box); they help players navigate SAB, and it would make for a appropriate cursor when hovering links and other clickable elements. A standard arrow cursor was needed to pair with with it, and a bit of code to make it work.

The basic idea is to hide the actual cursor, and have an element track with the mouse movement.

```css
/* hide the actual cursor */
body.customCursor {
    cursor: none;
}

/* the replacement cursor */
#cursor {
    display: none;
    position: fixed;
    width: 55px;
    height: 75px;

    background: url(//path-to-default-cursor.gif) no-repeat 0 0;
}
```

```js
var body       = document.querySelectorr("body"),
    cursor     = document.createElement("div"),
    pointerEls = "a, button";

cursor.setAttribute("id", "cursor");

// add the cursor element to the DOM
body.append(cursor);
// hide the actual cursor
body.setAttribute("class", "customCursor");

// update the position of the custom cursor when mousing around
body.addEventListener("mousemove", (e) => {
    // lock the position to a 10x10 grid for extra old school feel
    cursor.style.left = Math.floor(e.clientX / 10) * 10 + "px";
    cursor.style.top  = Math.floor(e.clientY / 10) * 10 + "px";
});
```

[Working example & code](http://codepen.io/kevinkace/pen/EWRBdX?editors=1111)

## MIDI

During the [Flash](https://en.wikipedia.org/wiki/Adobe_Flash) era, it was common to leveage [soundfonts](https://en.wikipedia.org/wiki/SoundFont) of a PC's soundcard to play notes via MIDI. This is no longer an option now that we are in a post-Flash world ([almost at least](http://www.theverge.com/2017/3/24/15052286/fedex-adobe-flash-five-dollar-discount-print-orders)). ArenaNet has a huge amount of [music from GW2 on SoundCloud](https://soundcloud.com/arenanet), and SoundCloud has [various APIs](https://developers.soundcloud.com/docs/api/html5-widget) for including and controlling audio on a site.

```js
var body   = document.body,
    iframe = document.createElement("iframe"),
    button = document.createElement("button"),
    player;

iframe.setAttribute("src", "//path-from-soundcloud-embed");
iframe.setAttribute("class", "hidden");

button.innerHTML = "click for music";

// add the button and iframe to the DOM
body.append(iframe);
body.append(button);

iframe.onload = function() {
    if(!SC) {
        return;
    }

    // to control the widget
    player = SC.Widget(iframe);
};

// click the button to begin playback
button.addEventListener("click", () => {
  if(player) {
    player.play();
  }
});
```

[Working example & code](http://codepen.io/kevinkace/pen/oZyrRy?editors=1111)

## Fonts

Comic. Sans.

What is there to say about [Comic Sans](https://en.wikipedia.org/wiki/Comic_Sans) that hasn't already been said?

It comes with all Windows PCs (and probably Windows Phone..?) as `"Comic Sans MS"`. Mac doesn't really have a great native fallback, so a [fallback](https://fonts.google.com/specimen/Coming+Soon?selection.family=Coming+Soon) is needed.

Add a dash of [Impact](https://en.wikipedia.org/wiki/Impact_(typeface)), and a low res font like [Square Pixel](http://www.dafont.com/pixel-square.font) for good measure. 

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Coming+Soon" type="text/css" media="all" />
```

```css
@font-face {
    font-family: "Pixel Square";
    src: url("/PixelSquare.eot");
    src: url("/PixelSquare.eot?#iefix") format("embedded-opentype"),
        url("/PixelSquare.woff") format("woff"),
        url("/PixelSquare.ttf") format("truetype");
    font-weight: normal;
    font-style: normal;
}

body {
    "Comic Sans MS", "Coming Soon", sans-serif;
}
```


## Progressively Loading Images

It's easy to complain about current internet speed, but it was once worse. Much, much worse. So of course we wanted to reproduce that! We liked the idea of reproducing progressive JPGs loading over a slow connection. There are various ways to get this effect.

1. GIF with progression as part animation
1. JPGs of steps in progression, animate with CSS
1. cripple server response speed/artificially increase JPGs to 10+MB for honest progression

GIFs max at 256 colors (in most situations), and in general are bigger and look worse than JPGs (depending on content). But no reason not to use that effect with previously rendered JPGs of steps of progression, and then animating with CSS.

```html
<div class="progImg" style="background-image: url(progressive-image-frames.jpg)">
    <img src="full-image.jpg">
</div>
```

```css
.progImg {
    background: no-repeat 0 100%;

    animation: progframes 1.5s steps(1) forwards;
}

.progImg img {
    display: block;
    opacity: 0;

    animation: progframe 1.5s forwards steps(1);
}

@keyframes progframes {
    0% {
        background-position: 0 100%;
    }
    /* remaining keyframes */
    83% {
        background-position: 0 0;
    }
    100% {
        background: none;
    }
}

@keyframes progframe {
    0% {
        opacity: 0;
    }
    100% {
        opacity: 1;
    }
}
```

## Tribulation Mode

Web browsers have come a long way. Todays browsers are incredibly powerful, and have fast release cycles, which pairs well with the rate of progress and evolution of website developement.

Early surfers of the web were not as lucky. Just talk with any web dev about Netscape Navigator 4, Internet Explorer 6, or any version of Safari. Websites used to be filled with rendering bugs. They still are, but they used to as well. We paired the idea of bad browser rendering with with [Tribulation Mode](https://wiki.guildwars2.com/wiki/Super_Adventure_Box:_Tribulation_Mode).

```js
var elements,
    numberOfElements = 50,
    firstRun = true,

    // styles to apply to random elements
    styleProps = [{
            name   : "display",
            values : [ "block", "inline" ]
        }, {
            name   : "position",
            values : [ "absolute", "relative", "fixed", "static" ]
        }, {
            name   : "margin",
            values : [ "-50px", "-20px", "-10px", "-5px", "5px", "10px", "20px", "50px" ]
        }, {
            name   : "padding",
            values : [ "-50px", "-20px", "-10px", "-5px", "5px", "10px", "20px", "50px" ]
        }, {
            name   : "width",
            values : [ "100%", "auto" ]
        }, {
            name   : "transform",
            values : [ "scale(1.3)", "scale(0.8)", "rotateZ(10deg)", "rotateZ(-10deg)" ]
        }],
    styleIdx = 0;

// get all elements on the page within the body, that aren't <script>
function getElements() {
    return document.querySelectorAll("body *:not(script)");
}

// get a random value of a style
function randomValue(values) {
    return values[ Math.floor(Math.random() * values.length) ];
}

// get a random element
function randomElement(els) {
    return els[ Math.floor(Math.random() * els.length) ];
}

// apply a random style to an element
function hack(element) {
    var propName = styleProps[styleIdx].name,
        value = randomValue(styleProps[styleIdx].values);

    element.style[propName] = value;

    styleIdx++;
    if(styleIdx >= styleProps.length) {
        styleIdx = 0;
    }
}

// apply styles to elements
function tribulation() {
    var idx;

    // hidden feature, so only get all elements if ran
    if(firstRun) {
        firstRun = false;
        elements = getElements();
    }

    // apply style to random elements
    for(idx = 0; idx < numberOfElements; idx++) {
        hack(randomElement(elements));
    }

    return "it's 1999!";
}

window.tribulation = window.TRIBULATION = tribulation;
```

Well that's it! Hope you enjoyed Super Adventure Festival, and this journey back in time.