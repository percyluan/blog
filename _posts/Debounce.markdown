---
layout: post
title:  "Debounce"
date:   2024-10-28 14:54:52
---

### What is debouncing? <br>
<p>Debouncing is a way of delaying the execution of a function until a certain amount of time has passed since the last time it was called.</p>


```javascript
// a high frequency event
// execute the function after the event stops for a certain time
function debounceSimple(fn, time) {
    let timeout = null;
    return function () {
        clearTimeout(timeout)
        timeout = setTimeout(() => {
            fn.apply(this,arguments)
        },time)
    }
}

// add trailing and leading option
function debounceWithOption(fn, time, options) {
    let timeout = null;
    let leadingInvoked = false;

    return function () {
        if(timeout) {
            clearTimeout(timeout)
        }
        // leading case
        if(options.leading && !timeout) {
            fn.apply(this,arguments);
            leadingInvoked = true
        } else {
            leadingInvoked = false
        }
        timeout = setTimeout(() => {
            //trailing case
            if(options.trailing && !leadingInvoked) {
                fn.apply(this,arguments)
            }
            timeout = null;
        },time)
    }
}
function getInputValue(event) {
    console.log("input value: " + event.data)
}
document.getElementById("inp-simple").addEventListener("input", debounceSimple(getInputValue, 3000))
document.getElementById("inp-option").addEventListener("input", debounceWithOption(getInputValue, 3000, {leading: true, trailing: true}))
```