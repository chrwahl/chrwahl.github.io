---
title:  "Scrolling Window"
date:   2021-10-12 11:28:00 +02
---

This is a basic example of updating a page based on scrolling behaviour.

The variable height is the max value found among the various heights on both window and body. And in the event listener, the scrolled percentage is calculated based on the current scrollY, the height and the height of the visible window.

```{.js}
var body = document.body,
  html = document.documentElement;

var height = Math.max(body.scrollHeight, body.offsetHeight,
  html.clientHeight, html.scrollHeight, html.offsetHeight);

document.addEventListener('scroll', e => {
  var percentY = window.scrollY / (height - window.innerHeight);
  document.querySelector('#box').style.opacity = percentY;
  document.querySelector('#number').textContent = Math.round(percentY*100);
});
```

A bit of CSS:

```{.css}
body {
  height: 1200px;
}
#box {
  height: 100px;
  width: 100px;
  position: fixed;
  background: navy;
  opacity: 0;
}
#number {
  position: fixed;
  background: navy;
  color: white;
}
```

A bit of HTML:

```{.html}
<div id="box"></div>
<div id="number">0</div>
```

Here is a working example in an iframe:

<iframe width="100%" height="200" src="data:text/html;base64,PGh0bWw+PGhlYWQ+PHNjcmlwdD5kb2N1bWVudC5hZGRFdmVudExpc3RlbmVyKCdET01Db250ZW50TG9hZGVkJywgZSA9PiB7dmFyIGJvZHkgPSBkb2N1bWVudC5ib2R5LGh0bWwgPSBkb2N1bWVudC5kb2N1bWVudEVsZW1lbnQ7dmFyIGhlaWdodCA9IE1hdGgubWF4KGJvZHkuc2Nyb2xsSGVpZ2h0LCBib2R5Lm9mZnNldEhlaWdodCwgaHRtbC5jbGllbnRIZWlnaHQsIGh0bWwuc2Nyb2xsSGVpZ2h0LGh0bWwub2Zmc2V0SGVpZ2h0KTtkb2N1bWVudC5hZGRFdmVudExpc3RlbmVyKCdzY3JvbGwnLCBlID0+IHt2YXIgcGVyY2VudFkgPSB3aW5kb3cuc2Nyb2xsWSAvIChoZWlnaHQgLSB3aW5kb3cuaW5uZXJIZWlnaHQpO2RvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoJyNib3gnKS5zdHlsZS5vcGFjaXR5ID0gcGVyY2VudFk7ZG9jdW1lbnQucXVlcnlTZWxlY3RvcignI251bWJlcicpLnRleHRDb250ZW50ID0gTWF0aC5yb3VuZChwZXJjZW50WSoxMDApO30pO30pOzwvc2NyaXB0PjxzdHlsZT5ib2R5IHtoZWlnaHQ6IDEyMDBweDt9ICNib3gge2hlaWdodDogMTAwcHg7d2lkdGg6IDEwMHB4O3Bvc2l0aW9uOiBmaXhlZDtiYWNrZ3JvdW5kOiBuYXZ5O29wYWNpdHk6IDA7fSAjbnVtYmVyIHtwb3NpdGlvbjpmaXhlZDsgYmFja2dyb3VuZDpuYXZ5O2NvbG9yOndoaXRlO308L3N0eWxlPjwvaGVhZD48Ym9keT48ZGl2IGlkPSdib3gnPjwvZGl2PjxkaXYgaWQ9J251bWJlcic+MDwvZGl2PjwvYm9keT48L2h0bWw+"></iframe>