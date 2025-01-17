# Events


References:  
[MDN Event reference](https://developer.mozilla.org/en-US/docs/Web/Events)  
[W3Schools HTML Dom Events](https://www.w3schools.com/jsref/dom_obj_event.asp)  

## Table of Contents

<!-- toc -->

- [Types of Events](#types-of-events)
  * [HTML5 Events](#html5-events)
- [Event Handling](#event-handling)
  * [Binding an event to an element](#binding-an-event-to-an-element)
  * [Passing arguments to event listener functions](#passing-arguments-to-event-listener-functions)
  * [Adding event listeners via a for loop](#adding-event-listeners-via-a-for-loop)
  * [Multiple identical event listeners](#multiple-identical-event-listeners)
  * [Removing event listeners](#removing-event-listeners)
- [Event Objects](#event-objects)
  * [Event object properties & methods](#event-object-properties--methods)
  * [Using the event object with other parameters](#using-the-event-object-with-other-parameters)
- [Event Delegation](#event-delegation)
  * [When unwanted child elements are the target](#when-unwanted-child-elements-are-the-target)
- [Debugging Event Listeners](#debugging-event-listeners)
- [Mutation Observers](#mutation-observers)
- [Examples](#examples)
  * [load, DOMContentLoaded](#load-domcontentloaded)
  * [focus & blur](#focus--blur)
  * [click](#click)
  * [keydown, keypress, keyup](#keydown-keypress-keyup)
  * [submit, change](#submit-change)

<!-- tocstop -->

## Types of Events

**UI Events**  
`load` - page has finished loading - fires on the window and document object  
`unload` - page is unloading (usually because a new page is requested) - fires on the body element node  
`error` - browser encounters a JavaScript error or missing asset  
`resize` - window has been resized  
`scroll` - user has scrolled up or down - this can relate to the entire page or a specific element on the page  

**Keyboard Events**  
`keydown` - user presses a key (repeats while key is pressed)  
`keyup` - user releases a key  
`keypress` - user presses a key that inserts an actual character (repeats...)  

**Mouse / Touch Events**  
`click` - user presses & releases or taps on an element   
`dblclick` - double click and releases or double-tap  
`contextmenu` - user right-clicks
`mousedown` - presses mouse, similar to touchstart  
`mouseup` - releases mouse, similar to touchend  
`mousemove` - moves the mouse (not applicable to touchscreen)  
`mouseover` - moves mouse over (not applicable to touchscreen)  
`mouseout` - moves mouse off (not applicable to touchscreen)  
`touchstart` - finger is placed on touchscreen  
`touchend` - finger is removed from touchscreen  
`touchmove` - finger dragged on touchscreen  
`touchcancel` - touch is interrupted   
`orientationchange` - device is rotated  

**Focus Events**  
`focus` - fires when an element gains focus  
`blur` - fires when an element looses focus  

**Form Events**  
`input` - value in an `<input>`, `<select>` or `<textarea>` has been changed
`change` - value in an `<input>`, `<select>` or `<textarea>` has changed  
`submit` - user submits a form  
`reset` - user resets a form  
`cut` - users cuts from a form field   
`copy` - users copies from a form field  
`paste` - users pastes in a form field  
`select` - user selects text in a form field   

**Animation & Transition Events**  
`animationend` - occurs when a CSS animation has ended  
`animationiteration` - occurs when a CSS animation is repeated  
`animationstart` - occurs when a CSS animation has started  
`transitionstart` - a CSS transition has started  
`transitionend` - a CSS transition has completed  
`transitioncancel` - a CSS transition has been canceled  
`transitionrun` - a CSS transition has begun running  


### HTML5 Events

`DOMContentLoaded` - this event fires when the DOM tree is formed (images, JavaScript and CSS may still be loading). While this runs before the `load` event, be aware that the DOM tree will not contain any HTML elements that would have been generated by other JavaScript.

`hashchange` - this event fires when the url hash changes (named anchors). The hashchange event works on the window object. After firing, the event object will have `oldURL` and `newURL` properties that hold the URL before and after the hashchange.

`beforeunload` - this event fires on the window object before the page is unloaded. It should only be used to help the user (not to encourage them to stay on the website). For example, it can be helpful to notify that changes on a form won't be saved.


## Event Handling

When a user interacts with the HTML on a page, there are three steps used to trigger JavaScript code. Together these steps are known as event handling.

1. State the function to run when the event occurs  
2. Select the event target: the element nodes you want the script to respond to (except for UI events that relate to the browser window)
3. Indicate which event will trigger the response (called *binding an event* to a DOM node)  


### Binding an event to an element  

As it turns out, there are three types of event handlers (ways to bind an event to an element).

**HTML Event Handlers** (old school).  
This outdated method of event handling is considered bad practice because it mixes JavaScript in with HTML code. It uses attributes and values to set events and functions on elements directly in the HTML. As with CSS, it's better to keep these things separated. For reference, here's what it looks like:

```html
<a onclick="myfunction()">not good</a>
```

**Traditional DOM Event Handlers** (meh)  
Though much better than HTML event handlers (it does separate JS from HTML), this method introduced in the original specification for the DOM still has a few drawbacks. Namely, you can only attach a single function to any one event. This can often be limiting. Here's an example:

```javascript
function checkUsername() {
  // code that checks something
}

let el = document.getElementById('username-field');

el.onblur = checkUsername;
```  

**DOM Level 2 Event Listeners** (preferred)  
Event Listeners allow for one event to trigger multiple functions. This is by far the favored way of handling events. It looks like this:

```javascript
function checkUsername() {
  let errorMsg = document.getElementById('username-error');
  if (this.value.length < 5) {
    errorMsg.textContent = 'Username must be at least 5 characters';
  } else {
    errorMsg.textContent = '';
  }
}

let el = document.getElementById('username-field');

el.addEventListener('blur', checkUsername, false);
```

Note that the other two methods prefix all the events with `on`, as in `onclick` or `onblur`. This method does not. It uses event names as they are.

The last boolean argument passed there is optional and has to do with event flow. Event flow is relevant in situations where an event occurs in an element inside another element, and both elements have registered a handle for that event. Event bubbling and capturing are two ways of event propagation in the HTML DOM API.

**Event Bubbling** - With bubbling, the event is first captured and handled by the innermost element and then propagated to outer elements (i.e. `a` -> `li` -> `ul` -> `body` -> `html` -> `document` ). Bubbling is the default (false) and probably what you will almost always use.  

**Event Capturing** - With capturing, the event is first captured by the outermost element and propagated to the inner elements. (i.e. `document` -> `html` -> `body` -> `ul` -> `li` -> `a` ).  

Another way to explain it would be: by setting this option to `true`, you are indicating that events of this type will be dispatched to the registered listener before being dispatched to any EventTarget *beneath* it in the DOM tree.

See [Bubbling and Capturing at javascript.info](https://javascript.info/bubbling-and-capturing) for a really good explantaion.


### Passing arguments to event listener functions

If you need to pass parameters to your function, you cannot wrap it with an anonymous function like so:

```javascript
function myFunction(p1, p2) {
  // demonstrating a function that takes arguments
  console.log(p1, p2);
}

let el = document.getElementById('username-field');

el.addEventListener('blur', function () {
  myFunction('hello', 'world');
}, false);
```


### Adding event listeners via a for loop

Many javascript developers recommend against adding event listeners to elements via a for loop. Their reasoning is problems can emerge when you try to use your `i` counter variable within the event callback (this however only appears to be a problem if you declare the variable with `var` instead of `let`). It's also apparently worse for performance. A better approach they say is through *event delegation* (described below). That being understood, the following example does work just fine:

```javascript
function processEvent() {
  console.log('doing something');
}

const els = document.querySelectorAll('.js-test');

for (let i = 0; i < els.length; i++) {
  els[i].addEventListener('click', processEvent, false);
}
```

To illustrate the problem when you try to use your `var i` counter variable within the event callback:

```javascript
const els = document.querySelectorAll('.js-test');

for (var i = 0; i < els.length; i++) {
  console.log('1', els[i]);
  els[i].addEventListener('click', function () {
    console.log('2', els[i]); // will log undefined
  }, false);
}
```
However, if you use `let`, everything works fine:

```javascript
const els = document.querySelectorAll('.js-test');

for (let i = 0; i < els.length; i++) {
  console.log('1', els[i]);
  els[i].addEventListener('click', function () {
    console.log('2', els[i]); // will log the element as expected
  }, false);
}
```

In terms of the second issue (performance), it feels like listening to every click in the document would be bad for performance, but it’s actually more performant than having a bunch of event listeners on individual items. Here's how we might turn the first for loop example above into an *event delegation* example:

```javascript
function processEvent() {
  console.log('doing something');
}

document.addEventListener('click', function (e) {
  if (e.target.matches('.js-test')) {
    processEvent()
  }
}, false);
```

`e.target` is a reference to the *element* that triggered the event and is described in its own section below. 

`matches()` is a method available to all *element objects* and is described in [document_object_model.md](document_object_model.md).


### Multiple identical event listeners

Note that there is an important difference between passing named functions and anonymous functions to event listeners and it has to do with duplicates. Normally, with a named function, if multiple identical EventListeners are registered on the same EventTarget with the same parameters, the duplicate instances are discarded. They do not cause the EventListener to be called twice, and they do not need to be removed manually with the `removeEventListener()` method (described below). When using an anonymous function however, such listeners will **not** be identical since anonymous functions are not identical even if defined using the same unchanging source-code. As a result you can end up with multiple event listeners on the same target if the function that contains the listener is being called repeatedly. This could in turn result in unexpected bugs and memory issues.

```javascript
/**
 * Example 1 (good)
 *
 * If initializeSomething() gets called more than once, NO duplicate
 * event listeners will be registered on the js-menu element.
 */
function processEvent() {
  // do something
}

function initializeSomething() {
  const el = document.querySelector('.js-menu');
  el.addEventListener('change', processEvent, false);
}
```

```javascript
/**
 * Example 2 (problematic)
 *
 * If initializeSomething() gets called more than once, duplicate
 * event listeners WILL be registered on the js-menu element.
 */
function initializeSomething() {
  const el = document.querySelector('.js-menu');
  el.addEventListener('change', function () {
    // do something
  }, false);
}
```

So what do you do if you need to pass arguments to your event handler function, but also prevent duplicates because the outer function may be called more than once? One solution could be through *event delegation* as described above in the for loop section (and also below). Another untested theory is to simply name the anonymous function.

As it's actually really hard to test this stuff (see [Debugging Event Listeners](#debugging-event-listeners) below), I'll have to test this at a later date and report back. 

TODO...


### Removing event listeners

There is a `removeEventListener()` method that removes an event listener from a target previously registered with `addEventListener()`. You need to specify the same type and listener parameters to removeEventListener() as on the original `addEventListener`. Calling `removeEventListener()` with arguments that do not match any currently registered event listeners on the event target has no effect.

```javascript
function doOnce(e) {
  // do something
  console.log('Doing something once.');
  e.target.removeEventListener('click', doOnce, false);
}

let el = document.getElementById('username-field');

el.addEventListener('click', doOnce, false);
```


## Event Objects

When an event occurs, the *event object* provides useful information about the event itself and the object it happened upon. This object is automatically passed to any function that is the event handler or listener. It has various properties and methods that can be accessed from the function, for example:

```javascript
// when referencing the event object, most programmers
// will use the parameter name: e, evt, or event

function logEventInfo(e) {
  // the full event object
  console.log(e);
  // event type (e.g. click)
  console.log(e.type);
  // the HTML element that triggered the event (and all its properties)
  console.log(e.target);
  // the HTML element that is handling the event (and all its properties)
  console.log(e.currentTarget);  
}

let el = document.getElementById('js-box');

el.addEventListener('click', logEventInfo, false);
```

The are different types of *event objects*, for example: `AnimationEvent`, `KeyboardEvent`, `MouseEvent`, `UiEvent`, `TouchEvent` and many more. They all inherit from the parent object: `Event`.

See also:  
[W3Schools DOM Event Objects](https://www.w3schools.com/jsref/obj_events.asp)  
[MDN Event Objects](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#Event_objects)  


### Event object properties & methods

Some of the properties and methods available on *event objects*:

`event.target` - The target property of the event object is always a reference to the *element* that the event has just occurred upon. You can use use this property to access any *element properties & methods* (see [document_object_model.md](document_object_model.md#working-with-elements)).

When calling a function, the event object's target property is the best way to determine which element the event occurred on. You can use this for *event delegation* described below. Note that you can *traverse the DOM* (see [document_object_model.md](document_object_model.md)) using `event.target`. For example, I could say `event.target.parentNode` or `event.target.nextElementSibling`. Here's a demo:

```javascript
function hideDropdown(e) {
  // This hides the dropdown menu when we click elsewhere
  let menuEl = document.querySelector('.js-dropdown-menu');
  if (e.target !== menuEl && e.target.parentElement !== menuEl) {
    menuEl.style.display = 'none';
  }
}
```

`event.currentTarget` -  Refers to the element to which the event handler has been attached, as opposed to `event.target`, which identifies the element on which the event occurred.  

`event.type` - The name of the event that was fired.  

`event.timeStamp` - the time (in milliseconds since epoch) that the event was created.  

`event.preventDefault()` - Cancels the default behaviour (if cancelable). For example, this can be useful when: clicking on a "submit" button to prevent it from submitting a form or clicking on a link to prevent the link from following the URL.  

`event.stopPropagation()` - Stops the event from *bubbling* or *capturing* any further along the DOM.  

Note that some event properties & methods are special to certain types of events, for example, the following properties are available to MouseEvents but not KeyboardEvents:

`event.screenX`, `event.screenY` - The screenX and screenY properties indicate the position of the cursor within the entire screen on your monitor (measuring from the top left corner).  

`event.pageX`, `event.pageY` - The pageX and pageY properties indicate the position of the cursor within the entire page. The top of the page may outside the viewport, so even if the cursor is in the same position, client and page coordinates can be different.

`event.clientX`, `event.clientY` - The clientX and clientY properties indicate the position within the browsers viewport. If the user has scrolled down, and the top of the page is no longer visible, this will have no affect on the coordinates.

[See MDN for full list of event objects, properties & methods.](https://developer.mozilla.org/en-US/docs/Web/API/Event)  


### Using the event object with other parameters

As we saw above, if you want to pass parameters to an event listener function, you wrap that function in an anonymous function. The event object in this case is automatically passed to the anonymous function. If you want to use its properties and methods in the inner named function, then you will have to label the parameter and pass it in. For example:

```javascript
function logEventInfo(e, p1, p2) {
  console.log(e.target);
  console.log(p1, p2);
}

let el = document.getElementById('js-box');

el.addEventListener('click', function (e) {
    logEventInfo(e, 'hello', 'world');
}, false);
```


## Event Delegation

Creating event listeners for many, many elements on a page, could slow performance down a bit by using a lot of memory. Event delegation is the idea of moving the event listening to one parent element and using the event objects target property to determine which element inside triggered the event.

```html
<ul id="main-list">
  <li>item one</li>
  <li>item two</li>
  <li>item three</li>
  <li>item four</li>
  <li>item five</li>
</ul>
```

```javascript
function deleteListItems() {
  // Select the container div
  let parent = document.getElementById('main-list');

  // Remove a list item
  function removeItem(e) {
    parent.removeChild(e.target);
  }

  // Create an event listener on the parent node
  parent.addEventListener('click', removeItem, false);
}

deleteListItems();
```

Keep in mind in the above example, if we had any other nested elements (like links, for example) we'd have to isolate the parent `li` as well as the grandparent `ul`. In the CSS we would need to ensure that the links were `display: block` so that we would be sure that they would be doing the triggering. Obviously this system won't work for every situation but is worth considering.

A better example is with *dynamically rendered elements*. Imagine a form where users can click to add additional fields, and click to remove them. With a traditional approach (attaching listeners to specific elements), you would need to add a new listener every time you added a field. With event delegation, you could setup your listener once and not have to think about when new elements are added, since it checks selectors at time of click rather than when the DOM is initially rendered. For example:

```javascript
/**
 * Adds a new div and html content to a container.
 *
 * This function adds a div to the parentEl arg, with the html content
 * provided by the html_content arg. The html_content should contain a button  
 * to be used for the delete functionality.
 */
function addContent(parentEl, html_content) {
  const newEl = document.createElement('div');
  newEl.innerHTML = html_content;
  parentEl.appendChild(newEl);
}

function deleteContent(e) {
  const groupEl = e.target.parentElement;
  groupEl.parentElement.removeChild(groupEl);
}


let parentEl = document.querySelector('.js-form');
let html_content = '<input type="text"> <button class="js-delete">×</button>';

document.addEventListener('click', function (e) {
  if (e.target.matches('.js-add')) {
    addContent(parentEl, html_content)
  }
  if (e.target.matches('.js-delete')) {
    deleteContent(e);
  }
}, false);
```

Again, this won't work for every situation. For example, some events only work with certain types of elements. If I wanted to add an event listener for a `change` event, that event only fires on an `<input>`, `<select>`, or `<textarea>` element. As a reminder, there's [a full list of events here](https://developer.mozilla.org/en-US/docs/Web/Events#Standard_events).


### When unwanted child elements are the target

When using event delegation, one thing we need to think about is whether any of our clickable elements have child elements. For example, consider the following:

```html
<button class="js-popup-btn">
  <img src="img/profile-placeholder.svg">
</button>
```

Using event delegation, my javascript might look something like this:

```javascript
window.addEventListener('click', function (e) {
  if (e.target.matches('.js-popup-btn')) {
    togglePopup();
  }
}, false);
```

What will happen here is the sometimes (maybe all the time), the `img` will end up being the `event.target` instead of the button.

> :bookmark: Remember: if we weren't doing event delegation and instead attached the event listener directly to the button, `e.currentTarget` would refer to the button and `e.target` would refer to the image. With event delegation, since we are attaching the event listener to the window, `e.currentTarget` isn't useful.

One easy way to prevent this (at least on buttons) is to add the following css:

```css
button > * {
  pointer-events: none;
}
```

Another way to prevent this is by using the `.closest()` method. basically, we need to not just check if the event's target matches, but also walk up the DOM tree in a simple simulation of bubbling. Here's an example:

```javascript
// Function to show/hide an element
function showHide(e) {
  const el = e.target.matches('.js-showhide') ? e.target : e.target.closest('.js-showhide');
  const detailsEl = el.parentElement.querySelector('.js-details');
  detailsEl.classList.toggle('is-hidden');
}

// Event listener
window.addEventListener('click', function (e) {
  if (e.target.matches('.js-showhide') || e.target.closest('.js-showhide')) {
  showHide(e);
  }
}, false);
```


## Debugging Event Listeners

In firefox, you can view which elements have event listeners applied to them right in the html panel. See [MDN web docs for instructions](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_event_listeners).

That being said, chrome arguably has some better tools. It also lets you see events on elements by going to the *Elements* panel, then looking for the *Event Listeners* sub-panel. In addition, it has [browser API methods](https://developers.google.com/web/tools/chrome-devtools/console/events) like `getEventListeners()` that can help monitor events. This code can be pasted into the console to list all events:

```javascript
function listListeners() {
    const items = Array.from(document.querySelectorAll('*')).map(element => {
      const listeners = getEventListeners(element);
      return {
        element: element,
        listeners: Object.keys(listeners).map(key => {
          return {
            event: key,
            listeners: listeners[key]
          };
        })
      };
    }).filter(item => item.listeners.length);

    // log them to the console
    console.log(items);

    // put a border around the elements
    items.forEach(function (item) {
      item.element.style.outline = '1px solid red';
    });
}

listListeners();
```

To make things super frustrating though, both Firefox and Chrome show all duplicate instances of event listeners on a target element even though MDN says [duplicate identical EventListeners registered on the same EventTarget are discarded](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Multiple_identical_event_listeners).

TODO... will have to look into this further.


## Mutation Observers

The `MutationObserver` interface provides the ability to watch for changes being made to the DOM tree. It is designed as a replacement for the older *Mutation Events* feature. [Details can be found here](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver).


```javascript  
// Select the node that will be observed for mutations
let targetNode = document.getElementsByClassName('js-list')[0];

// Select the element that will respond to the mutations
let updateNode = document.getElementsByClassName('js-total')[0];

// Select the element that will add elements
let addItemNode = document.getElementsByClassName('js-add')[0];

// Set options for the observer (which mutations to observe)
let config = { attributes: true, childList: true, subtree: true };

// Callback function to execute when mutations are observed
let updateCount = function (mutationsList, observer) {
  for (let mutation of mutationsList) {
    if (mutation.type == 'childList') {
      updateNode.textContent = targetNode.childElementCount;
    } else if (mutation.type == 'attributes') {
      console.log('The ' + mutation.attributeName + ' attribute was modified.');
    }
  }
};

// A function to add elements so we have something to observe
function addItem(e) {
  e.preventDefault();
  newItem = document.createElement('li');
  newText = document.createTextNode('New item');
  newItem.appendChild(newText);
  targetNode.appendChild(newItem);
}

// Event listener for adding items
addItemNode.addEventListener('click', addItem, false);

// Create an observer instance linked to the callback function
let observer = new MutationObserver(updateCount);

// Start observing the target node for configured mutations
observer.observe(targetNode, config);

// Later, you can stop observing
// observer.disconnect();
```


## Examples


### load, DOMContentLoaded

Apparently the DOM Level 2 specification says that `load` fires on the document object. Prior to this, if fired on the window object. Browsers support both and most developers still use the window object (unconfirmed).

Here's an example that uses the load event to put focus on a given form input element so the user can immediately start typing:

```javascript
function setup() {
  let firstInput = document.getElementById('user_or_email');
  firstInput.focus();
}

window.addEventListener('load', setup, false);
```

This alternate solution gives focus to the first input element (and assumes it has a label first) once the page is loaded.

```javascript
function setup() {
  let formElement = document.querySelector('form.js-form');
  let firstInput = formElement.firstElementChild.nextElementSibling;
  firstInput.focus();
}

window.addEventListener('load', setup, false);
```

Keep in mind that the `load` event only fires once **everything** else on the page has loaded. So if, for example, you had a bunch of video and images, the user could (in theory) already be partway through the form by the time it forces focus to the first field.. super annoying! A better solution in this case would be to use the HTML5 event `DOMContentLoaded`:

```javascript
window.addEventListener('DOMContentLoaded', setup, false);
```


### focus & blur

These can be useful in working with forms. For example if you want to provide tips or feedback to users as they interact with an element in a form. They can also be used to trigger form validation as a user moves from one field to the next rather that waiting for the submit button. For example:

```javascript
// these functions provide tips and validate a field

function infoUsername() {
  elMsg.className = 'auth-form__info';
  elMsg.innerHTML = 'Username should be at least five characters';
}

function checkUsername() {
  let username = elUsername.value;
  if (username.length < 5) {
    elMsg.className = 'auth-form__error';
    elMsg.textContent = 'Not long enough!'
  } else if (existingUsers.includes(username)) {
    elMsg.className = 'auth-form__error';
    elMsg.textContent = 'Sorry, that username is taken!';
  } else {
    elMsg.textContent = '';
  }
}

let existingUsers = ['robert', 'timothy', 'susan'];
let elMsg = document.getElementsByClassName('js-messages')[0];
let elUsername = document.getElementById('user_or_email');

elUsername.addEventListener('focus', infoUsername, false);
elUsername.addEventListener('blur', checkUsername, false);
```


### click

This example demonstrates adding HTML content and using the click event:

```javascript
let msg = '<div class="announcement__header">';
msg += '<a href="#" class="announcement__close-btn js-close">x close</a></div>';
msg += '<div class="announcement__msg">';
msg += '<h2 class="announcement__heading">System Maintenance</h2>';
msg += '<p>Our servers will be updated between 3 and 5 am PST. ';
msg += 'During this time, you may experience minor disruptions. ';
msg += 'We do this periodically to ensure everything runs smooth.</p></div>';

let elAnnouncement = document.createElement('div');

elAnnouncement.setAttribute('class', 'announcement');
elAnnouncement.innerHTML = msg;
document.body.appendChild(elAnnouncement);

function dismissAnnouncement() {
  document.body.removeChild(elAnnouncement);
}

let elClose = document.getElementsByClassName('js-close')[0];

elClose.addEventListener('click', dismissAnnouncement, false);
```


### keydown, keypress, keyup

Note that the `key...` events fire in that order. `keydown` and `keypress` events have a `keyCode` property which reports what key was pressed as an ASCII code. To convert it to the character, you can use the `String` objects built in method `fromCharCode()`. See the example below:

```javascript
function charCount(e) {
  let textEntered = document.getElementsByClassName('js-textbox')[0].value;
  let charDisplay = document.getElementsByClassName('js-counter')[0];
  let counter = (180 - textEntered.length);
  let lastKey = document.getElementsByClassName('js-lastkey')[0];

  charDisplay.textContent = counter;
  lastKey.textContent = String.fromCharCode(e.keyCode);
}

let el = document.getElementsByClassName('js-textbox')[0];
el.addEventListener('keyup', charCount, false);
```


### submit, change

While the submit event is pretty self-explanatory, the change event fires when:
 - a selection is made from a select menu
 - a radio button is selected
 - a checkbox is selected or deselected

Change events are often more appropriate than click events as users may interact with form elements by tab, arrow or enter keys. The example below shows the *sets* field if the *reps* value is selected in *units*. On submit it also checks that a checkbox is selected.

```javascript
let elForm = document.getElementsByClassName('js-form')[0];
let elUnits = document.getElementById('units');
let elSets = document.getElementById('sets');
let elHuman = document.getElementById('human');
let elMsg = document.getElementsByClassName('js-messages')[0];

function showSets() {
  let unit = this.options[this.selectedIndex].value;
  if (unit === 'reps') {
    elSets.removeAttribute('disabled');
    elSets.previousElementSibling.classList.remove('disabled');
  } else {
    elSets.setAttribute('disabled', 'true');
    elSets.value = '';
    elSets.previousElementSibling.classList.add('disabled');
  }
}

function checkHuman(e) {
  if (!elHuman.checked) {
    elMsg.textContent = 'Please confirm that you are indeed a human.';
    elMsg.className = 'js-messages activity-form__error';
    e.preventDefault();
  } else {
    elMsg.textContent = 'Thanks!';
    elMsg.className = 'js-messages activity-form__info';
  }
}

elUnits.addEventListener('change', showSets, false);
elHuman.addEventListener('change', checkHuman, false);
elForm.addEventListener('submit', checkHuman, false);
```
