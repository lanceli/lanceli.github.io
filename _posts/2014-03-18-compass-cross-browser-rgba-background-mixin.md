---
layout: post
title:  "Compass cross-browser RGBa background mixin"
date:   2014-03-18 09:39:22 +0800
categories: css
---

For more information about pure css background transparency, check this [http://robertnyman.com/2010/01/11/css-background-transparency-without-affecting-child-elements-through-rgba-and-filters/](https://web.archive.org/web/20141202154801/http://robertnyman.com/2010/01/11/css-background-transparency-without-affecting-child-elements-through-rgba-and-filters/)

## SCSS

```scss
// Provides cross-browser RGBa background.
// Takes a RABa color as the argument, e.g. rgba(0, 0, 0, 0.5) for black background with 50% opacity.
//
//     @param $color
//         A RGBa color
@mixin rgba-background($color){
    @include filter-gradient($color, $color);
    @if $legacy-support-for-ie6 or $legacy-support-for-ie7 or $legacy-support-for-ie8 {
        background: transparent;
 
        // set filter as none for IE9+, because IE9+ supprot RGBa
        :root & {
        filter: none\0/IE9;}
    }
    background: $color;
}
 
.blackAlpha50 {
    @include rgba-background(rgba(0, 0, 0, .5));
}
```



## CSS	

```css
.blackAlpha50 {
    *zoom: 1;
    filter: progid:DXImageTransform.Microsoft.gradient(gradientType=0, startColorstr='#80000000', endColorstr='#80000000');
    background: transparent;
    background: rgba(0, 0, 0, 0.5);
}
:root .blackAlpha50 {
    filter: none\0/IE9;
}
```



Gist: [https://gist.github.com/lanceli/3248619](https://gist.github.com/lanceli/3248619)
