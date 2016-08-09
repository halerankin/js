# Top 10* jQuery Tasks Using Native JS

This represents a limited selection of commonly used jQuery functions and the native JavaScript (JS) eqivalent. Each topic contains: sample markup where necessary, jQuery implementation and return value, JS implementation and return value, browser compatibility and a link to MDN (Mozilla Developer Network) detaiing the key native JS method used. Please note: browser compatibility listed in this presentation is not meant to be comprehensive. For more details on implementation of a given native JS method, review its use and notes on compatibility in the related MDN page.

## Topics
- Selector
- Parent
- Offset Parent
- Add Class
- Remove Class
- Has Class
- Hide/Show
- Looping
- Get Text
- Remove
- Append
- Event handler on
- Event handler off
- Get JSON
- Parse JSON

---


## Markup
The following markup is used for all but two examples:

```html
<div class="membership-plans">
  <ul class="mobile-accordion-list">
    <li class="featured">
      <h3><a href="/#" class="accordion-trigger">Plenty<span class="offscreen">: minimize content</span></a></h3>
      <span class="featured-label"> <span>Optimum Value </span></span>               
      <div class="accordion-content" style="">
            <p class="plan-subhead">OUR MOST FAVORITE THING</p>
            <p>This is a line of text.</p>
            <p class="plan-price">
                <sup>$</sup><span>900.25</span> per year
            </p>
            <a href="#" class="btn btn-red">Look Now</a>
        </div>
    </li>
    <li class="">
      <h3><a href="/#" class="accordion-trigger">Not Plenty<span class="offscreen">: minimize content</span></a></h3>
      <span class=""> <span> </span></span>               
      <div class="accordion-content" style="">
            <p class="plan-subhead">BASIC THING</p>
            <p>This is another line of text.</p>
            <p class="plan-price">
                <sup>$</sup><span>0.50</span> per year
            </p>
            <a href="#" class="btn btn-red">Look Now</a>
        </div>
    </li>
    <li class="">…</li>
    <li class="">…</li>
    <li class="">…</li>
    <li class="plan-promo">…</li>
  </ul>
</div>
```>
```

---

## Selectors

```javascript
.membership-plans                 // parent container for membership plans
.membership-plans>ul>li           // individual membership plan
.membership-plans>ul>li.featured  // featured membership plan
```


## DOM traversal / manipulation
### Selector
Select all membership plans. 
Select featured plan.

jQuery:

```javascript
$('.membership-plans>ul>li');

// returns: [<li>, <li class="featured">, <li>, <li>, <li>, <li class="plan-promo">] (6)

$('.membership-plans>ul>li.featured');

// returns: [<li class="featured">] (1)
```

JS:

```javascript
document.querySelectorAll('.membership-plans>ul>li');

// returns: NodeList [<li>, <li class="featured">, <li>, <li>, <li>, <li class="plan-promo">] (6)

document.querySelector('.membership-plans>ul>li.featured');

// returns: <li class="featured">…</li>
```

**Browser Compatibiity:** IE8+

**Takeaway** -- [document.querySelectorAll()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll), [document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)


---


### Parent
Select parent of feaured membership item.

jQuery:

```javascript
$('.membership-plans>ul>li.featured').parent();

// returns: [<ul class="mobile-accordion-list">] (1)
```

JS:

```javascript
document.querySelector('.membership-plans>ul>li.featured').parentNode;

// returns: <ul class="mobile-accordion-list">…</ul>
```

**Browser Compatibiity:** IE8+

**Takeaway** -- [node.parentNode](https://developer.mozilla.org/en-US/docs/Web/API/Node/parentNode)


---


### Offset Parent
Select offset parent of content inside membership item.

jQuery:

```javascript
$('.membership-plans>ul>li.featured h3').offsetParent();

// returns: [<li class="featured">] (1)

$('.membership-plans>ul>li h3').offsetParent();

// returns: [<li>, <li class="featured">, <li>, <li>, <li>, <li class="plan-promo">] (6)
```

JS:

```javascript
document.querySelector('.membership-plans>ul>li.featured h3').offsetParent;

// returns: <li class="featured">…</li>

document.querySelector('.membership-plans>ul>li h3').offsetParent;

// returns: <li class>…</li> (parent of first instance of selector)

document.querySelectorAll('.membership-plans>ul>li h3').offsetParent;

// returns: undefined
```

Note: Using jQuery, it is possible to get the list of parents corresponding to the jQuery node collection. As seen above, when using JS this will not work reliably as document.querySelector returns just the first instance of the selector, and document.querySelectorAll returns a node list, not a single node. In order to return the parentNode of each node in the list, you would have to loop through this list using _Array.prototype.forEach_. More on this below.

**Browser Compatibiity:** IE9+

**Note:**  [from MDN] This property will return null on Webkit if the element is hidden (the style.display of this element or any ancestor is "none") or if the style.position of the element itself is set to "fixed".

**Takeaway** -- [HTMLElement.offsetParent](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/offsetParent)


---


### Add Class
Adding class 'flag' to featured list item.

jQuery:

```javascript
var customClass = 'flag',
	$featuredMembership = $('.membership-plans>ul>li.featured');

$featuredMembership.addClass(customClass);

// returns: [<li class="featured flag">] (1)
```

JS:
Look out! There might be multiple classes on the element. You will need to check for existing classes and add the new one to it without replacing the others.

```javascript
var customClass = 'flag',
	elFeaturedMembership = document.querySelector('.membership-plans>ul>li.featured');

if (elFeaturedMembership.classList) {
	elFeaturedMembership.classList.add(customClass);
} else {
	elFeaturedMembership.className += ' ' + customClass;
}
elFeaturedMembership = document.querySelector('.membership-plans>ul>li.featured');

// returns: <li class="featured flag">…</li>
```

**Browser Compatibiity:** IE8+

**Takeaway** -- [element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList)


---


### Remove Class
Removing recently added class 'flag' from featured list item.

jQuery:

```javascript
var customClass = 'flag',
	$featuredMembership = $('.membership-plans>ul>li.featured');

$featuredMembership.removeClass(customClass);

// returns: [<li class="featured">] (1)
```

JS:
Look out! There might be multiple classes on the element. You will need to check for existing classes and remove only the one.

```javascript
var customClass = 'flag',
	elFeaturedMembership = document.querySelector('.membership-plans>ul>li.featured');

if (elFeaturedMembership.classList) {
	elFeaturedMembership.classList.remove(customClass);
} else {
	elFeaturedMembership.className = elFeaturedMembership.className.replace(new RegExp('(^|\\b)' + customClass.split(' ').join('|') + '(\\b|$)', 'gi'), ' ');
}
elFeaturedMembership = document.querySelector('.membership-plans>ul>li.featured');

// returns: <li class="featured">…</li>
```

**Browser Compatibiity:** IE8+

**Takeaway** -- [element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList), [RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)


---


### Has Class
Check to see if element has a given class. 

jQuery:

```javascript
var customClass = 'featured',
	$featuredMembership = $('.membership-plans>ul>li.featured');

$featuredMembership.hasClass(customClass);

// returns: true
```

JS:

```javascript
var customClass = 'featured',
	elFeaturedMembership = document.querySelector('.membership-plans>ul>li.featured');

if (elFeaturedMembership.classList) {
	elFeaturedMembership.classList.contains(customClass);
} else {
	new RegExp('(^| )' + customClass + '( |$)', 'gi').test(elFeaturedMembership.className);
}

// returns: true
```

**Browser Compatibiity:** IE8+

**Takeaway** -- [element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList), [RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)


---


### Hide/Show	
Hide or show an element.

jQuery:

```javascript
var $featuredMembership = $('.membership-plans>ul>li.featured');

$featuredMembership.hide();

// hides element and returns: [<li class="featured">] (1)

$featuredMembership.show();

// shows element and returns: [<li class="featured">] (1)
```

JS:

```javascript
var elFeaturedMembership = document.querySelector('.membership-plans>ul>li.featured');

elFeaturedMembership.style.display = 'none';

// hides element and returns: "none"

elFeaturedMembership.style.display = '';

// shows element and returns: "";
```

Note: If element is already hidden via CSS using `display: none`, you cannot force it to appear using the above JS. In this case, you would have to provide a value for display:
```javascript
elFeaturedMembership.style.display = 'block';
```

**Browser Compatibiity:** IE8+

**Takeaway** -- [HTMLelement.style](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style)


---


### Looping 
Loop over multiple returned elements.

jQuery:

```javascript
$('.membership-plans>ul>li').each( function(index, element) {
	console.log(index + ' ' + element.tagName);
});

// returns:
[Log] 0 LI
[Log] 1 LI
[Log] 2 LI
[Log] 3 LI
[Log] 4 LI
[Log] 5 LI
[<li>, <li class="featured">, <li>, <li>, <li>, <li class="plan-promo">] (6)
```

JS:

```javascript
var memberships = document.querySelectorAll('.membership-plans>ul>li');

Array.prototype.forEach.call(memberships, function(element, index){
	console.log('index: ' + index.value + ', element: ' + element.tagName);
});

// returns: 
[Log] index: 0, element: LI
[Log] index: 1, element: LI
[Log] index: 2, element: LI
[Log] index: 3, element: LI
[Log] index: 4, element: LI
[Log] index: 5, element: LI
```

**Browser Compatibiity:** IE9+

**Takeaway** -- [Array.prototype.forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)


---

### Get Text
Return all text from inside the featured membership plan item.

targeted element:
```html
<li class="featured">
  <h3><a href="/#" class="accordion-trigger">Plenty<span class="offscreen">: minimize content</span></a></h3>
  <span class="featured-label"> <span>Optimum Value </span></span>                
  <div class="accordion-content" style="">
        <p class="plan-subhead">OUR MOST FAVORITE THING</p>
        <p>This is a line of text.</p>
        <p class="plan-price">
            <sup>$</sup><span>900.25</span> per year
        </p>
        <a href="#" class="btn btn-red">Look Now</a>
    </div>
</li>
```

jQuery:

```javascript
var $featuredMembership = $('.membership-plans>ul>li.featured');

$featuredMembership.text();

// returns: (all text inside all elements within list item)
"
      Plenty: minimize content
       Optimum Value                
      
            OUR MOST FAVORITE THING
            This is a line of text.
            
                $900.25 per year
            
            Look Now       
"

$('.membership-plans>ul>li.featured').find('h3').text();

// returns: "Plenty: minimize content"
```

JS:

```javascript
var elFeaturedMembership = document.querySelector('.membership-plans>ul>li.featured');

elFeaturedMembership.textContent;

// returns: (all text inside all elements within list item)
"
      Plenty: minimize content
       Optimum Value                
      
            OUR MOST FAVORITE THING
            This is a line of text.
            
                $900.25 per year
            
            Look Now       
"

document.querySelector('.membership-plans>ul>li.featured h3').textContent;

// returns: "Plenty: minimize content"
```

**Browser Compatibiity:** IE9+

**Takeaway** -- [node.textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)

---


### Remove
Remove element from the DOM.

jQuery:

```javascript
var $featured = $('.membership-plans>ul>li.featured');

$featured.remove();

// returns: [<li class="featured">] (1)
```

JS:

```javascript
var elFeatured = document.querySelector('.membership-plans>ul>li.featured');

elFeatured.parentNode.removeChild(elFeatured);

// returns: <li class="featured">…</li>
```

**Browser Compatibiity:** IE8+

**Takeaway** -- [node.RemoveChild](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild)


---


### Append
Append existing element to the DOM. Removes element from existing place in the DOM and appends it to the given parent element as the last of its existing child nodes. 

jQuery:

```javascript
var $membershipContainer = $('.membership-plans>ul'),
    $featured = $('.membership-plans>ul>li.featured');

$membershipContainer.append($featured);

// returns: [<ul class="mobile-accordion-list">] (1)
```

JS:

```javascript
var elMembershipContainer = document.querySelector('.membership-plans>ul'),
    elFeatured = document.querySelector('.membership-plans>ul>li.featured');

elMembershipContainer.appendChild(elFeatured);

// returns: <li class="featured">…</li>
```

**Browser Compatibiity:** IE8+

**Takeaway** -- [node.AppendChild](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild)


---



## Events
### Event handler on
Assign click handler to each membership plan (LI).

jQuery:

```javascript
var $memberships = $('.membership-plans>ul>li');

$memberships.on( 'click', onMembershipClick );

function onMembershipClick(event) {
  console.log('clicked', event.target.offsetParent);
  console.log('clicked currentTarget', event.currentTarget);
}

// returns: 
[<li>, <li class="featured">, <li>, <li>, <li>, <li class="plan-promo">] (6)
// when an item is clicked, returns: 
[Log] clicked – <div class="container">…</div>
[Log] clicked currentTarget – <li class>…</li>
```

JS:

```javascript
var memberships = document.querySelectorAll('.membership-plans>ul>li');

Array.prototype.forEach.call(memberships, function(index, element){
	index.addEventListener( 'click', onMembershipClick);
});

function onMembershipClick(event) {
  console.log('clicked', event.target.offsetParent);
  console.log('clicked currentTarget', event.currentTarget);
}

// returns: undefined
// when an item is clicked, returns: 
[Log] clicked – <div class="container">…</div>
[Log] clicked currentTarget – <li class>…</li>
```

Note: The above implementation includes both `event.target.offsetParent` and `event.currentTarget` to show that it's possible to return an element outside what was intended. 

**Browser Compatibiity:** IE9+ (You will need a polyfill for older browsers.)

**Takeaway** -- [element.addEventListener / element.attachEvent](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)


---


### Event handler off
Remove click handler from each membership plan (LI).


jQuery:

```javascript
$memberships.off( 'click', onMembershipClick );

// returns: [<li>, <li class="featured">, <li>, <li>, <li>, <li class="plan-promo">] (6)
```

JS:

```javascript
Array.prototype.forEach.call(memberships, function(index, element){
	index.removeEventListener( 'click', onMembershipClick);
});

// returns: undefined
```

**Browser Compatibiity:** IE9+ (Note: You will need a polyfill for older browsers.)

**Takeaway** -- [element.removeEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener)


---


## AJAX / JSON

### Sample data
data.json
``` json
{
  "one":    "dodo",
  "two":    "giraffe",
  "three":  "manatee"
}
```

data-string.js
```json
{"one":"dodo","two":"giraffe","three":"manatee"}
```

### Request
Request JSON data.

jQuery:

```javascript
$.getJSON('data.json', function(data) {
  console.log(data);
})
.done(function() {
  console.log( "second success" );
})
.fail(function() {
  console.log( "error" );
})
.always(function() {
  console.log( "complete" );
});

// returns: 
[Log] {one: "dodo", two: "giraffe", three: "manatee"}
[Log] second success
[Log] complete
```

JS:

```javascript
var request = new XMLHttpRequest();
request.open('GET', 'data.json', true);

request.onload = function() {
  if (request.status >= 200 && request.status < 400) {
    // Success!       
    var data = request.responseText;
    console.log(data);
  } else {
    // We reached our target server, but it returned an error
    console.log('connected, but error');
  }
};

request.onerror = function() {
  // There was a connection error of some sort
  console.log('connection error');
};

request.send();

// returns: 
[Log] "{↵  \"one\":    \"dodo\",↵  \"two\":    \"giraffe\",↵ \"three\":  \"manatee\"↵}" 
```

_That output needs to be cleaned up!_ **See below**

**Browser Compatibiity:** (some properties IE8+, others IE10+)

**Takeaway** -- [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)


---


### Parse JSON
Retrieve string and parse JSON.

jQuery:

```javascript
$.get('data-string.js', function(data) {
  console.log('Response: ', data);
  var parsedResponse = $.parseJSON(data);
  console.log('Parsed response: ', parsedResponse);
}); 

// returns:
Response:  {"one":"dodo","two":"giraffe","three":"manatee"} 
Parsed response:  Object { one: "dodo", two: "giraffe", three: "manatee" } 
```

JS:

```javascript
var request = new XMLHttpRequest();

request.open('GET', 'data-string.js', true);

request.onload = function() {
  if (request.status >= 200 && request.status < 400) {
    var responseText = request.responseText;
    console.log('Response: ', responseText);
    var parsedText = JSON.parse(responseText)
    console.log('Parsed: ', parsedText);
  }
};
request.send();

// returns: 
Response:  {"one":"dodo","two":"giraffe","three":"manatee"}
Parsed:  Object { one: "dodo", two: "giraffe", three: "manatee" }
```

**Browser Compatibiity:** IE8+

**Takeaway** -- [JSON.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)


---


## References:
[YouMightNotNeedjQuery](http://youmightnotneedjquery.com)
[MDN](https://developer.mozilla.org/en-US/)

*okay, so fifteen.
