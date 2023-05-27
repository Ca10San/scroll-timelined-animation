# scroll-timelined-animation
**Animate while you scroll the page with PURE JS!**

Do you like animated webpages like Apple do in their flagship hardware, but you didn't know how that was made?

Do you already worked in a project that was focused on maximum performance yet beautiful visuals?

## If you just need the code:
**[IT'S HERE!](./example_code.html)**

But if you want to understand what is happening here and even adapt it to your needs, [I will explain everything on this video](/) and in the text below.

## How does that works ?

Starting from the beggining you need to understand what is that parameters

```
document.addEventListener('DOMContentLoaded', function () {  
    instantiateAnimation(
        'animationWrapper1',
        1400,
        1950, 
        34, 
        'http://localhost/img/image_example'
    );
});
```

First I add a **event listener** in the DOM, that will be triggered when the **DOM Content is Loaded**. It calls an function that instantiates the animation structure in the page, but it needs:

* the ID Selector of the element which will wrap all
* where in the page, the animation will starts up
* where in the page the animation will ends
* how many frames do your animation have
* the base URL which the function will iterate and set on the children nodes

## The animation wrapper

Is the first info which you will need to insert in the function, there's no secrets here, just put something like the tag below in your html document:

`<div id="animationWrapper1"></div>`

If you want to add more animations in your page just add more wrappers and point them in the trigger.

```
<div id="animationWrapper2"></div>
<div id="animationWrapper3"></div>
<div id="animationWrapper4"></div>
<div id="animationWrapper5"></div>
```

```
document.addEventListener('DOMContentLoaded', function () {  
    instantiateAnimation(
        'animationWrapper2',
        2100,
        2600, 
        34, 
        'http://localhost/img/any-other-image_example-'
    );

    instantiateAnimation(
        'animationWrapper3',
        2800,
        3400, 
        34, 
        'http://localhost/img/any-other-image_example-'
    );
    
    instantiateAnimation(
        'animationWrapper4',
        3600,
        4000, 
        34, 
        'http://localhost/img/any-other-image_example-'
    );
    
    instantiateAnimation(
        'animationWrapper5',
        4100,
        4600, 
        34, 
        'http://localhost/img/any-other-image_example-'
    );
});
```

Just dont forget the many animations you have the more media your page will download, and it maybe turn all this useless.

## The page offset

Are the 2nd & 3rd info you will need, and it's how you get them.

Just copy this code, place it in your console, and scroll your page:

```
window.addEventListener('scroll', () => { 
    console.log(window.pageYOffset);
});
```

You will see some data printed on your console, that numbers reflect the position your screen is within the document. You'll just have to see where your div is on your page and copy where it begins and where it ends. On the first example my animation wrapper was near the mid document's length, so the animation had to begin at the position 1400 and end at 1950.


## The frames amount

You can use how many frames do you want in order to make your animation smoothiest as possible, so just don't you forget about the data usage, [here you'll see some example frames](./img).

You just have to point the exact same frames number you will use, in this example I used 34 frames.

## The base URL

Like the example images, all of them need to use the same name AND extension having no difference except the index number at the end on the link to where your images is being served.

## Need some more technical explanation?
[IT'S HERE!](./docs/more_technical_explanation.md)