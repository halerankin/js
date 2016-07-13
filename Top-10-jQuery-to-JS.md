# Top 10 jQuery Tasks Using Native JS

This presentation of jQuery and native JS features use the following markup for all but two examples:

**_Note: all links open in a new window._**

###Url
[AAAWA QA - Membership Benefits Overview](http://aaawa-qa.popworldwide.com/membership/benefits-overview){:target="_blank"}

###Markup
```HTML
<div class="membership-plans">
  <ul class="mobile-accordion-list">
    <li class="featured">
      <h3><a href="/Membership/Plans/Plus" class="accordion-trigger">Plus<span class="offscreen">: minimize content</span></a></h3>
      <span class="featured-label"> <span>Best Value </span></span>               
      <div class="accordion-content" style="">
            <p class="plan-subhead">OUR MOST POPULAR PLAN</p>
            <p>Perfect for anyone who wants AAA's broadest, best-value coverage, for people commuting more than 5 miles to work or for those who do a lot of traveling in their cars.</p>
            <p class="plan-price">
                <sup>$</sup><span>92.75</span> per year
            </p>
            <a href="https://secure-dev.aaawa.com/membermanagement/join/plans/plus" class="btn btn-red">Join Now</a>
        </div>
    </li>
    <li class="">
      <h3><a href="/Membership/Plans/Classic" class="accordion-trigger">Classic<span class="offscreen">: minimize content</span></a></h3>
      <span class=""> <span> </span></span>               
      <div class="accordion-content" style="">
            <p class="plan-subhead">BASIC COVERAGE FOR ALL</p>
            <p>Perfect for anyone who wants AAA's coverage, for people commuting more than 5 miles to work or for those who do a lot of traveling in their cars.</p>
            <p class="plan-price">
                <sup>$</sup><span>59.00</span> per year
            </p>
            <a href="https://secure-dev.aaawa.com/membermanagement/join/plans/classic" class="btn btn-red">Join Now</a>
        </div>
    </li>
    <li class="">...</li>
    <li class="">...</li>
    <li class="">...</li>
    <li class="plan-promo">...</li>
  </ul>
</div>
```

###Selectors
```javascript
.membership-plans>ul>li
.membership-plans>ul>li.featured
```

###Topics
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


##
## DOM traversal / manipulation
### Selector	
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

**Takeaway** -- [document.querySelectorAll()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll){:target="_blank"}, [document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector){:target="_blank"}


##
### Parent
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

**Takeaway** -- [node.parentNode](https://developer.mozilla.org/en-US/docs/Web/API/Node/parentNode){:target="_blank"}


##
### Offset Parent
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

Note: Using jQuery, it is possible to get the list of parents corresponding to the jQuery node collection. As seen above, when using JS this will not work reliably as document.querySelector returns just the first instance of the selector, and document.querySelectorAll returns a node list, not a single node. In order to return the parentNode of each node in the list, you would have to loop through this list using _Array.prototype.forEach_.

**Takeaway** -- [HTMLelement.offsetParent](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/offsetParent){:target="_blank"}


##
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
**Takeaway** -- [element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList){:target="_blank"}


##
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
**Takeaway** -- [element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList){:target="_blank"}, [RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp){:target="_blank"}


##
### Has Class
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
**Takeaway** -- [element.classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList){:target="_blank"}, [RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp){:target="_blank"}


##
### Hide/Show	
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

**Takeaway** -- [HTMLelement.style](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style){:target="_blank"}


##
### Looping 
Looping over multiple returned elements.

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
**Takeaway** -- [Array.prototype.forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach){:target="_blank"}


##
### Get Text

targeted element:
```HTML
<li class="featured">
	<h3><a href="/Membership/Plans/Plus" class="accordion-trigger">Plus<span class="offscreen">: minimize content</span></a></h3>
  <span class="featured-label"> <span>Best Value </span></span>								
  <div class="accordion-content" style="">
        <p class="plan-subhead">OUR MOST POPULAR PLAN</p>
        <p>Perfect for anyone who wants AAA's broadest, best-value coverage, for people commuting more than 5 miles to work or for those who do a lot of traveling in their cars.</p>
        <p class="plan-price">
            <sup>$</sup><span>92.75</span> per year
        </p>
        <a href="https://secure-dev.aaawa.com/membermanagement/join/plans/plus" class="btn btn-red">Join Now</a>
    </div>
</li>
```

jQuery:	
```javascript
var $featuredMembership = $('.membership-plans>ul>li.featured');

$featuredMembership.text();

// returns: (all text inside all elements within list item)
"
      Plus: minimize content
       Best Value 								
      
            OUR MOST POPULAR PLAN
            Perfect for anyone who wants AAA's broadest, best-value coverage, for people commuting more than 5 miles to work or for those who do a lot of traveling in their cars.
            
                $92.75 per year
            
            Join Now       
"

$('.membership-plans>ul>li.featured').find('h3').text();

// returns: "Plus: minimize content"
```

JS:
```javascript
var elFeaturedMembership = document.querySelector('.membership-plans>ul>li.featured');

elFeaturedMembership.textContent;

// returns: (all text inside all elements within list item)
"
      Plus: minimize content
       Best Value 								
      
            OUR MOST POPULAR PLAN
            Perfect for anyone who wants AAA's broadest, best-value coverage, for people commuting more than 5 miles to work or for those who do a lot of traveling in their cars.
            
                $92.75 per year
            
            Join Now       
"

document.querySelector('.membership-plans>ul>li.featured h3').textContent;

// returns: "Plus: minimize content"
```

**Takeaway** -- [node.textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent){:target="_blank"}

##
### Remove

##
### Append

##
## Events

### Event handler on
jQuery:
```javascript
var $memberships = $('.membership-plans>ul>li');

$memberships.on( 'click', onMembershipClick );

function onMembershipClick(event) {
	console.log('clicked', event.target.offsetParent);
  console.log('clicked currentTarget', event.currentTarget);
}

// returns: [<li>, <li class="featured">, <li>, <li>, <li>, <li class="plan-promo">] (6)
// when an item is clicked, returns: clicked – <li class>…</li>
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
// when an item is clicked, returns: clicked – <li class>…</li>
```

Note: You will need a polyfill for older browsers.

**Takeaway** -- [element.addEventListener / element.attachEvent](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener){:target="_blank"}


### Event handler off
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

Note: You will need a polyfill for older browsers.

**Takeaway** -- [element.removeEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/removeEventListener){:target="_blank"}


##
## AJAX / JSON
### Request
jQuery:
```javascript
$.getJSON('http://localhost:3333/data.json', function(data) {
	console.log( data );
});
```

JS:
```javascript
var request = new XMLHttpRequest();
request.open('GET', '/my/url', true);

request.onload = function() {
  if (request.status >= 200 && request.status < 400) {
    // Success!
    var data = JSON.parse(request.responseText);
  } else {
    // We reached our target server, but it returned an error

  }
};

request.onerror = function() {
  // There was a connection error of some sort
};

request.send();
```

##
### Parse JSON
jQuery:
```javascript
$.parseJSON(string);
```

JS:
```javascript
JSON.parse(string);
```


##
##References:
[YouMightNotNeedjQuery](http://youmightnotneedjquery.com){:target="_blank"}
[MDN](https://developer.mozilla.org/en-US/){:target="_blank"}


