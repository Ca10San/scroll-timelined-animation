# A liitle bit more technical explanation

## instantiateAnimation

```
function instantiateAnimation(
    animationWrapperIDSelector, 
    minPageOffset, 
    maxPageOffset, 
    framesAmount,
    framesBaseURL
) {
    const animationWrapper = document.getElementById(animationWrapperIDSelector);

    if(animationWrapper) {
        if(!animationWrapper.className.includes('animationWrapper')) {
            animationWrapper.className = 'animationWrapper';
        }
        const deltaOffset = (maxPageOffset - minPageOffset) / framesAmount;

        const framesWrapper = buildFramesWrapper(framesAmount, framesBaseURL);
        animationWrapper.appendChild(framesWrapper);

        const framesWidth = animationWrapper.getBoundingClientRect().width;
        const maxWrapperOffset = calculateMaxOffset(framesAmount, framesWidth);

        window.addEventListener('scroll', function () {
            if(
                (window.pageYOffset > minPageOffset) 
                && (window.pageYOffset < maxPageOffset)
            ) {               
                updateImage(framesWrapper, minPageOffset, maxWrapperOffset, deltaOffset, framesWidth);
            } else {
            }
        })
    } else {
        console.log(`animation wrapper ${animationWrapperIDSelector} not found`);
    }
}
```

After finding the wrapper and added the correct className (if it doesn't already have) it calculates the **delta**.

### deltaOffset

The **max** and **min** page offset sets a range where the animation begins and where it ends, and that **animation range** is divided by **frames amount**. Keep this in mind, it will be **important in the future**.

## Frames wrapper

```
function buildFramesWrapper(framesAmount, framesBaseURL) {    
    const framesWrapper = document.createElement('div');
    framesWrapper.className = 'framesWrapper';
    const instantiatedFrames = instantiateFrames(framesAmount, framesBaseURL);

    for(let index = 0; index < instantiatedFrames.length; index++) {
        framesWrapper.appendChild(instantiatedFrames[index]);
    }

    return framesWrapper;
}
```

```
function instantiateFrames(framesAmount, framesBaseURL) {
    let frames = [];

    for (let index = 1; index <= framesAmount; index++) {
        let newFrame = document.createElement('img');
        newFrame.className = 'animationFrame';        
        newFrame.src = framesBaseURL + index + '.jpg';

        frames.push(newFrame);
    }
    
    return frames;
}
```

I think that doesn't have any secret here, creates an element, add the respective className and appends the frames array returned by **instantiateFrames()** and each image is loaded based on **base URL**  interpolated with index entry. (If you want to use PNG instead of JPEG, just change the '.jpg' at the end of the line by '.png' or any other extension you may want use).

### framesWidth
**It's very important**, cause this info will define the jump's size between frames translation.

### maxWrapperOffset
Defines where the animation ends, it's important because when the scrolling continues down the page and if theresn't a max offset value, the animation illusion is **broken**, the frames wrapper disappear and everything stops working appropriately.

## updateImage()

**HERE IS WHERE THE MAGIC HAPPENS!**
```
updateImage(framesWrapper, minPageOffset, maxWrapperOffset, deltaOffset, framesWidth);
```

```
function updateImage(framesWrapper, minPageOffset, maxWrapperOffset, deltaOffset, framesWidth) {    
    let frameOffset = calculateCurrentOffset(minPageOffset, deltaOffset, framesWidth);
    framesWrapper.style.setProperty('transform', `translateX(${ frameOffset > maxWrapperOffset ? frameOffset : maxWrapperOffset }px)`);
}

 function calculateCurrentOffset(minPageOffset, deltaOffset, framesWidth) {
    let positionOffset =  Math.floor((window.pageYOffset - minPageOffset) / deltaOffset);
    return frameOffset = (positionOffset * framesWidth) * -1;
}

```

Do you remember of **deltaOffset** ? We're gonna use it right now, the **frameOffset** is calculated based on the minimum view offset, so the math is very simple **pageYOffset - minPageOffset**  defines the actual position of the wrapper on the page view, divided by the **deltaOffset** previously calculated it basically returns the frame index that will be shown but a float number doesnt fit to find and array index so the **Math.floor()** solves the problem returning an integer, but this it has to calculate the next frame translation, multiplying the earlier calculated **positionOffset** by **frames width** we reach the exact position we have to translate the row view to show the correct frame.
(That -1 means to turn the solved math into a negative version of itself to correctly translate in X axis forwards and backwards).


# Bonus info

If you need an 'sticky animation', you can simply add a new css class or something like this on **frames wrapper**:
```
.animation_fixed {
    position: -webkit-sticky;
    position: sticky;
    top: 0;
}
```


But it's important to say, you'll need to change the **animation wrapper height** so the **frames wrapper** will sticky on top of the screen.

You can modify by yourself the CSS to centralize the animation while it's stuck on top of the screen until the end of the **animation div**, after that you will need to find again the **maxPageOffset** and set on the animation instantiation.