---
title:  "SVG Metro Map"
date:   2021-09-10 14:00:00 +02
---

One of my [resent answers on Stackoverflow](https://stackoverflow.com/a/69123547/322084) was about styling a HTML `<process>` element like a metro map (at least, that was my interpretation...). OK, is is not possible to style a HTML element like that, so I went for a version in SVG.

The finally result looks like this:

<script>
document.addEventListener('DOMContentLoaded', e => {
  document.forms.form01.range.addEventListener('change', e => {
    let numlines = parseInt(e.target.value);
    let numdots = (numlines < 1) ? 0 : numlines+1;
    document.querySelector('#styles').innerHTML = `
      .lines use:nth-child(-n+${numlines}) {
        stroke: DarkSlateBlue;
      }
      .dots use:nth-child(-n+${numdots}) {
        stroke: DarkSlateBlue;
      }`;
  });
});
</script>

<style>
  #line line {
    stroke-width: 6px;
  }

  #dot circle {
    stroke-width: 3px;
    fill: white;
  }

  #down path, #up path {
    stroke-width: 6px;
    fill: none;
  }

  svg use {
    stroke: SteelBlue;
  } 
</style>

<svg viewBox="0 0 250 50">
  <defs>
    <symbol id="line">
      <line x1="0" y1="3" x2="40" y2="3" />
    </symbol>
    <symbol id="dot" transform="translate(-6 -3)">
      <circle cx="6" cy="6" r="4" fill="white" />
    </symbol>
    <symbol id="down">
      <path  transform="translate(0 3)" d="M 0,0 C 25,0 15,20 40,20" />
    </symbol>
    <symbol id="up">
      <path  transform="translate(0 3)" d="M 0,20 C 25,20 15,0 40,0" />
    </symbol>
  </defs>
  <style id="styles"></style>
  <g class="lines">
    <use href="#line" transform="translate(10 10)"/>
    <use href="#line" transform="translate(50 10)"/>
    <use href="#down" transform="translate(90 10)"/>
    <use href="#up"   transform="translate(130 10)"/>
    <use href="#line" transform="translate(170 10)"/>
  </g>
  <g class="dots">
    <use href="#dot" transform="translate(10 10)"/>
    <use href="#dot" transform="translate(50 10)"/>
    <use href="#dot" transform="translate(90 10)"/>
    <use href="#dot" transform="translate(130 30)"/>
    <use href="#dot" transform="translate(170 10)"/>
    <use href="#dot" transform="translate(210 10)"/>
  </g>
</svg>

<form name="form01">
  <input type="range" name="range" min="0" max="5" value="0"/>
</form>

This is basically the SVG and HTML code. I use `<symbol>` to create the different elements for the metro map and then `<use>` for placing these elements on the map. It is a quite flexible way to go about it.

```html
<svg viewBox="0 0 250 50">
  <defs>
    <symbol id="line">
      <line x1="0" y1="3" x2="40" y2="3" />
    </symbol>
    <symbol id="dot" transform="translate(-6 -3)">
      <circle cx="6" cy="6" r="4" fill="white" />
    </symbol>
    <symbol id="down">
      <path  transform="translate(0 3)" d="M 0,0 C 25,0 15,20 40,20" />
    </symbol>
    <symbol id="up">
      <path  transform="translate(0 3)" d="M 0,20 C 25,20 15,0 40,0" />
    </symbol>
  </defs>
  <style id="styles"></style>
  <g class="lines">
    <use href="#line" transform="translate(10 10)"/>
    <use href="#line" transform="translate(50 10)"/>
    <use href="#down" transform="translate(90 10)"/>
    <use href="#up"   transform="translate(130 10)"/>
    <use href="#line" transform="translate(170 10)"/>
  </g>
  <g class="dots">
    <use href="#dot" transform="translate(10 10)"/>
    <use href="#dot" transform="translate(50 10)"/>
    <use href="#dot" transform="translate(90 10)"/>
    <use href="#dot" transform="translate(130 30)"/>
    <use href="#dot" transform="translate(170 10)"/>
    <use href="#dot" transform="translate(210 10)"/>
  </g>
</svg>

<form name="form01">
  <input type="range" name="range" min="0" max="5" value="0"/>
</form>
```
I added a bit of JavaScript for making it dynamic. When the range `<input>` is changed I will update a smaller part of the CSS.

```javascript
document.forms.form01.range.addEventListener('change', e => {
  let numlines = parseInt(e.target.value);
  let numdots = (numlines < 1) ? 0 : numlines+1;
  document.querySelector('#styles').innerHTML = `
    .lines use:nth-child(-n+${numlines}) {
      stroke: DarkSlateBlue;
    }
    .dots use:nth-child(-n+${numdots}) {
      stroke: DarkSlateBlue;
    }`;
});
```
The CSS:

```css
#line line {
  stroke-width: 6px;
}

#dot circle {
  stroke-width: 3px;
  fill: white;
}

#down path, #up path {
  stroke-width: 6px;
  fill: none;
}

svg use {
  stroke: SteelBlue;
}
```
