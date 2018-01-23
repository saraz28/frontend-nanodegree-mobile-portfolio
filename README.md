Frontend-nanodegree-mobile-portfolio
============
Optimize online portfolio for speed, optimize the critical rendering path and make this page render as quickly as possible by applying techniques.

## index.html
Achieves a PageSpeed score at least 90 for Mobile and Desktop. First of all, installed ngrok to make local server running through PageSpeed and here the screenshots of scores and what i do to optimize it:
# Desktop:
![alt text](https://b.top4top.net/p_753306xk2.png)
# Mobile:
![alt text](https://a.top4top.net/p_7532lkmf1.png)

* When it's no need to block rendering until we use it so i added a media query on print.
```bash
<link href="css/print.css" rel="stylesheet" media="print">
```
* Adding async.
```bash
<script async src="http://www.google-analytics.com/analytics.js"></script>
```
* optimizing Web Fonts.
```bash
<link href='http://netdna.bootstrapcdn.com/font-awesome/4.0.3/css/font-awesome.css' rel='stylesheet'>
```
* Optimizing images.
* Inline CSS.
* Minify CSS & Javascript.
* Minify HTML.
* Removing inline script at the end of th body.
## views/js/main.js
* Removed _scrollTop_ to above of _for loop_.
* Create variable scroll.
* Change querySelectorAll and querySelector to getElementsByClassName and getElementById.

Original:
```bash
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.querySelectorAll('.mover');
  for (var i = 0; i < items.length; i++) {
    // document.body.scrollTop is no longer supported in Chrome.
    var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
    var phase = Math.sin((scrollTop / 1250) + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }
```
After changes:
```bash
 var items = document.getElementsByClassName('mover');
  var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
  var scroll = Math.sin(scrollTop / 1250);
  for (var i = 0; i < items.length; i++) {
    // document.body.scrollTop is no longer supported in Chrome.
    var phase = scroll + (i % 5);
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }
  ```
* Creating variable instead of repeating it many times which cause alot of work.
* Removing both _dx_, _newwidth_ above of _for loop_ to reduce the time of resize pizzas.

Original:
```bash
  function changePizzaSizes(size) {
    for (var i = 0; i < document.querySelectorAll(".randomPizzaContainer").length; i++) {
      var dx = determineDx(document.querySelectorAll(".randomPizzaContainer")[i], size);
      var newwidth = (document.querySelectorAll(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
      document.querySelectorAll(".randomPizzaContainer")[i].style.width = newwidth;
    }
  }
  ```
  After changes:
  ```bash
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.getElementsByClassName('mover');
  var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
  var scroll = Math.sin(scrollTop / 1250);
  for (var i = 0; i < items.length; i++) {
    // document.body.scrollTop is no longer supported in Chrome.
    var phase = scroll + (i % 5);
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }
```

Original:

```bash
function changeSliderLabel(size) {
    switch(size) {
      case "1":
        document.querySelector("#pizzaSize").innerHTML = "Small";
        return;
      case "2":
        document.querySelector("#pizzaSize").innerHTML = "Medium";
        return;
      case "3":
        document.querySelector("#pizzaSize").innerHTML = "Large";
        return;
      default:
        console.log("bug in changeSliderLabel");
    }
  }
```
After changes:

```bash
  var PizzaS = document.getElementById("pizzaSize");
  function changeSliderLabel(size) {
    switch(size) {
      case "1":
        PizzaS.innerHTML = "Small";
        return;
      case "2":
        PizzaS.innerHTML = "Medium";
        return;
      case "3":
        PizzaS.innerHTML = "Large";
        return;
      default:
        console.log("bug in changeSliderLabel");
    }
  }
```

