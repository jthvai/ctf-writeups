# Leggos (beginner, 100pt)

_Solved on 18 Sep. 2020._

## Statement

> I <3 Pasta! I won't tell you what my special secret sauce is though!
>
> <https://chal.duc.tf:30101>
>
> Author: Crem

## Solution

Head to the challenge site on a browser, and open developer tools. (I realised after the challenge that the page won't
let you right click; for most browsers, this is achievable through `Ctrl+Shift+I`, or a menu option.) Expand the header
of the HTML, and this `<script>` tag is revealed.

```html
<script src="disableMouseRightClick.js"></script>
```

Switch to the Sources tab, and open `disableMouseRightClick.js`.

```js
document.addEventListener('contextmenu', function(e) {
    e.preventDefault();
    alert('not allowed');
});

  //the source reveals my favourite secret sauce
  // DUCTF{n0_k37chup_ju57_54uc3_r4w_54uc3_9873984579843}
```
