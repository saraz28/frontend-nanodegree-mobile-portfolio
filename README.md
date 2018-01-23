Website Performance Optimization portfolio project
============
Optimize online portfolio for speed, optimize the critical rendering path and make this page render as quickly as possible by applying techniques.
## Getting Started
first of all clone or download it as a zip file. 
1. To run your local server on your phone 
```
$> cd /path/to/your-project-folder
$> python -m SimpleHTTPServer 8080
```
2. Download and install [ngrok](https://ngrok.com/3) to a locally running web service.
```
$> cd /path/to/your-project-folder
$> ./ngrok http 8080
```
Then Copy the public URL ngrok gives you as shown in the screenshot and try running it through [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/).
![alt text](https://f.top4top.net/p_75373j901.png)

## index.html
Achieves a PageSpeed score at least 90 for Mobile and Desktop. First of all, installed ngrok to make local server running through PageSpeed and here the screenshots of scores and what i do to optimize it:
# Desktop:
![alt text](https://a.top4top.net/p_753xpj812.png)
# Mobile:
![alt text](https://b.top4top.net/p_753vc1bo3.png)

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
* Removed _scrollTop_ to above of _for loop_ so the frame render at 60fps and no jank appear.
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
  var scroll = scrollTop / 1250 + (i % 5);
  for (var i = 0; i < items.length; i++) {
    // document.body.scrollTop is no longer supported in Chrome.
    var phase = Math.sin(scroll);
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }
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
Original:
Remove pizzasDiv above of _for loop_.
```bash
for (var i = 2; i < 100; i++) {
  var pizzasDiv = document.getElementById("randomPizzas");
  pizzasDiv.appendChild(pizzaElementGenerator(i));
}
```
After changes:

```bash
var pizzasDiv = document.getElementById("randomPizzas");
for (var i = 2; i < 100; i++) {
  pizzasDiv.appendChild(pizzaElementGenerator(i));
}
```
Calculate the number of pizzas needed to fill the screen, based on browser window resolution. 
Original:
```bash
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  for (var i = 0; i < 200; i++) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "images/pizza.png";
    elem.style.height = "100px";
    elem.style.width = "73.333px";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    document.querySelector("#movingPizzas1").appendChild(elem);
  }
  updatePositions();
});

```

After changes:

```bash
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  var rows =  (window.screen.height / s)* cols;
  for (var i = 0; i < rows; i++) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "images/pizza.png";
    elem.style.height = "100px";
    elem.style.width = "73.333px";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    document.getElementById("movingPizzas1").appendChild(elem);
  }
  updatePositions();
});
```
