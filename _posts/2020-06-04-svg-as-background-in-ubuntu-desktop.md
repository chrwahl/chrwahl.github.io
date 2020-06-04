---
title:  "SVG as background in Ubuntu Desktop"
date:   2020-06-04 08:50:00
tags: ["Ubuntu","Gnome","SVG"]
---

Ok, this is not something new, but I just installed [Ubuntu 20.04](https://ubuntu.com/blog/ubuntu-20-04-lts-arrives){:target="_blank"} on my new laptop and in that process I dusted off my background image for the desktop.

Since Windows 2000 I have always use a neutral background for my desktop: The blue color on Windows and later a solid blue color on Ubuntu with Unity. And since Ubuntu switched to Gnome an image with either a solid blue color or a gradient.

**The** blue background:

<div style="background-color:#3A6EA5;width:240px;height:135px"></div>

After the switch to Gnome I quickly found out that a SVG could be used as a background image. This is my current version:


```xml
<?xml version="1.0"?>
<svg viewBox="0 0 16 9" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <linearGradient id="g1" gradientTransform="rotate(90)">
      <stop offset="0" stop-color="SteelBlue"/>
      <stop offset="1" stop-color="Black"/>
    </linearGradient>
  </defs>
  <rect width="16" height="9" fill="url(#g1)"/>
</svg>
```
![](data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIj8+Cjxzdmcgdmlld0JveD0iMCAwIDE2IDkiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CiAgPGRlZnM+CiAgICA8bGluZWFyR3JhZGllbnQgaWQ9ImcxIiBncmFkaWVudFRyYW5zZm9ybT0icm90YXRlKDkwKSI+CiAgICAgIDxzdG9wIG9mZnNldD0iMCIgc3RvcC1jb2xvcj0iU3RlZWxCbHVlIi8+CiAgICAgIDxzdG9wIG9mZnNldD0iMSIgc3RvcC1jb2xvcj0iQmxhY2siLz4KICAgIDwvbGluZWFyR3JhZGllbnQ+CiAgPC9kZWZzPgogIDxyZWN0IHdpZHRoPSIxNiIgaGVpZ2h0PSI5IiBmaWxsPSJ1cmwoI2cxKSIvPgo8L3N2Zz4K){:width="240px"}

I like the simplicity: Setting the aspect ratio of the screen in the view box, using named colors for the gradient, and setting the rectangle to fill the entire space.