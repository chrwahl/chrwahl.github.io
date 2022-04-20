---
title:  "SVG gauge with threshold"
date:   2022-03-06 16:30:00 +02
---

Based on a question asked on StackOverflow I made this example of a gauge in SVG.

I think that this is my first example where I make use of the `pathLength` attribute on a `<circle>`.
Not that it is magic, but I find it more clean than turning a `<path>` into a circle.
The threshold doesn't do anything in this example, but it is a nice feature that the text flips then passing 50% (6 o'clock).

### JavaScript

```{.js}
var meter = document.getElementById('meter');
var threshold = document.getElementById('threshold');

document.forms.form01.addEventListener('change', e => {
  let newval = e.target.value;
  switch(e.target.name){
    case 'val':
      meter.setAttribute('stroke-dasharray', `${newval} 100`);
      break;
    case 'tval':
      threshold.setAttribute('transform', `translate(50 50) rotate(-90) rotate(${3.6*newval})`);
      let text = threshold.querySelector('text');
      text.textContent = newval;
      let transformStr = 'translate(43 -1)';
      if(newval > 50) transformStr += ' rotate(180) translate(-5 -2)';
      text.setAttribute('transform', transformStr);
      break;
  }
});
```

### HTML


```{.html}
<form name="form01">
<lable>Value: <input name="val" type="range" min="0" max="100" value="25"/></lable>
<lable>Threshold: <input name="tval" type="range" min="0" max="100" value="33"/></lable>
</form>
<svg width="300" viewBox="0 0 100 100" fill="none" stroke-width="5" stroke-linecap="round">
  <circle stroke="silver" cx="50" cy="50" r="40"/>
  <circle id="meter" transform="rotate(-90 50 50)" stroke="green" cx="50" cy="50" r="40"
  pathLength="100" stroke-dasharray="25 100"/>
  <g id="threshold" transform="translate(50 50) rotate(-90) rotate(120)">
    <line x1="37.5" y1="0" x2="47.5" y2="0" stroke="black" stroke-width="1"/>
    <text transform="translate(43 -1)" font-size="4" fill="black">33</text>
  </g>
</svg>
```

### Example

Here is a working example in an iframe:

<iframe width="100%" height="350" src="data:text/html;base64,PGh0bWw+CjxoZWFkPgo8c2NyaXB0Pgpkb2N1bWVudC5hZGRFdmVudExpc3RlbmVyKCdET01Db250ZW50TG9hZGVkJywgZSA9PiB7CnZhciBtZXRlciA9IGRvY3VtZW50LmdldEVsZW1lbnRCeUlkKCdtZXRlcicpOwp2YXIgdGhyZXNob2xkID0gZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoJ3RocmVzaG9sZCcpOwoKZG9jdW1lbnQuZm9ybXMuZm9ybTAxLmFkZEV2ZW50TGlzdGVuZXIoJ2NoYW5nZScsIGUgPT4gewogIGxldCBuZXd2YWwgPSBlLnRhcmdldC52YWx1ZTsKICBzd2l0Y2goZS50YXJnZXQubmFtZSl7CiAgICBjYXNlICd2YWwnOgogICAgICBtZXRlci5zZXRBdHRyaWJ1dGUoJ3N0cm9rZS1kYXNoYXJyYXknLCBgJHtuZXd2YWx9IDEwMGApOwogICAgICBicmVhazsKICAgIGNhc2UgJ3R2YWwnOgogICAgICB0aHJlc2hvbGQuc2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nLCBgdHJhbnNsYXRlKDUwIDUwKSByb3RhdGUoLTkwKSByb3RhdGUoJHszLjYqbmV3dmFsfSlgKTsKICAgICAgbGV0IHRleHQgPSB0aHJlc2hvbGQucXVlcnlTZWxlY3RvcigndGV4dCcpOwogICAgICB0ZXh0LnRleHRDb250ZW50ID0gbmV3dmFsOwogICAgICBsZXQgdHJhbnNmb3JtU3RyID0gJ3RyYW5zbGF0ZSg0MyAtMSknOwogICAgICBpZihuZXd2YWwgPiA1MCkgdHJhbnNmb3JtU3RyICs9ICcgcm90YXRlKDE4MCkgdHJhbnNsYXRlKC01IC0yKSc7CiAgICAgIHRleHQuc2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nLCB0cmFuc2Zvcm1TdHIpOwogICAgICBicmVhazsKICB9Cn0pOwp9KTsKPC9zY3JpcHQ+CjwvaGVhZD4KPGJvZHk+Cjxmb3JtIG5hbWU9ImZvcm0wMSI+CjxsYWJsZT5WYWx1ZTogPGlucHV0IG5hbWU9InZhbCIgdHlwZT0icmFuZ2UiIG1pbj0iMCIgbWF4PSIxMDAiIHZhbHVlPSIyNSIvPjwvbGFibGU+CjxsYWJsZT5UaHJlc2hvbGQ6IDxpbnB1dCBuYW1lPSJ0dmFsIiB0eXBlPSJyYW5nZSIgbWluPSIwIiBtYXg9IjEwMCIgdmFsdWU9IjMzIi8+PC9sYWJsZT4KPC9mb3JtPgo8c3ZnIHdpZHRoPSIzMDAiIHZpZXdCb3g9IjAgMCAxMDAgMTAwIiBmaWxsPSJub25lIiBzdHJva2Utd2lkdGg9IjUiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCI+CiAgPGNpcmNsZSBzdHJva2U9InNpbHZlciIgY3g9IjUwIiBjeT0iNTAiIHI9IjQwIi8+CiAgPGNpcmNsZSBpZD0ibWV0ZXIiIHRyYW5zZm9ybT0icm90YXRlKC05MCA1MCA1MCkiIHN0cm9rZT0iZ3JlZW4iIGN4PSI1MCIgY3k9IjUwIiByPSI0MCIgcGF0aExlbmd0aD0iMTAwIiBzdHJva2UtZGFzaGFycmF5PSIyNSAxMDAiLz4KICA8ZyBpZD0idGhyZXNob2xkIiB0cmFuc2Zvcm09InRyYW5zbGF0ZSg1MCA1MCkgcm90YXRlKC05MCkgcm90YXRlKDEyMCkiPgogICAgPGxpbmUgeDE9IjM3LjUiIHkxPSIwIiB4Mj0iNDcuNSIgeTI9IjAiIHN0cm9rZT0iYmxhY2siIHN0cm9rZS13aWR0aD0iMSIvPgogICAgPHRleHQgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoNDMgLTEpIiBmb250LXNpemU9IjQiIGZpbGw9ImJsYWNrIj4zMzwvdGV4dD4KICA8L2c+Cjwvc3ZnPgo8L2JvZHk+CjwvaHRtbD4K"></iframe>



