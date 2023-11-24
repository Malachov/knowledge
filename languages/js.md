# Js reference

This is also reference for TypeScript in own typing section

## TOC

<!-- TOC -->
* [Js reference](#js-reference)
  * [TOC](#toc)
  * [New](#new)
  * [Syntax](#syntax)
    * [Misc](#misc)
  * [Conditions](#conditions)
    * [HTML interaction](#html-interaction)
<!-- TOC -->

## Syntax

### Misc
```js
// Ternary operator

var isadult = (age < 18) ? 'tooyoung': 'old enough';

var exports = {
  publicPath: process.env.NODE_ENV === 'production'
    ? '/production-sub-path/'
    : '/'
}

// Optional Chaining

// When foo is defined, foo.bar.baz() will be computed; but when foo is null or undefined, stop what we’re doing and just return undefined
let x = foo?.bar.baz();

// If object with param not available, use default value

transfersDataSource?.numberOfItems || 0
```

## Conditions
```js
if (1 < 2) {
    alert("jo")
}

switch (key) {
    case value:
        
        break;

    default:
        break;
}
```

### HTML interaction
```html

<!--Create new elements and populate it-->
<div id="demo">some content</div>

<script>
    // Creating a new element
    var p = document.createElement("p");
    var node = document.createTextNode("Some new text");

    // Adding the text to the paragraph
    p.appendChild(node);

    // Getting the element 
    var div = document.getElementById("demo");

    // Adding the paragraph to the div
    div.appendChild(p);

    // Changing image src
    document.getElementById("qr").src = "/images/new"

    // Remove classes from all elements
    function make_button_visible() {

        var elems = document.getElementsByClassName('nonactive');

        while (elems.length > 0) {
            elems[0].classList.remove('nonactive');
        }
    }
</script>
```
