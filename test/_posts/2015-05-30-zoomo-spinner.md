---
layout: post
title:  "The Zoomo Logo Spinner Deconstructed"
date:   2015-05-30 23:25:05
author: Ananth
categories: css3 animation front-end design spinner 
permalink: "/zoomo-logo-spinner-deconstructed"
image: true
---

Making your logo a loading spinner isn't new. It makes for good branding and allows you a nice excuse to flash your company's logo. Every once in a while, we come across an example that conveys the extraordinary amount of care and detail that goes into making a good spinner great. One such example is the spinner that Slack uses for their web application. So, we thought we'd make something with ours.

![Zoomo spinner](/img/zoomo-spinner.gif)

**We recommend keeping the CodePen example open while reading the tutorial.** <a href="http://codepen.io/zoomodesign/pen/BNLxYO" target="_blank"><b>Click here to edit in CodePen.</b></a>

This is our logo.

![Zoomo logo](/img/zoomo-logo.jpg)

It's simple, and it has immense recall value. If you look closely, you'll see that the logo isn't comprised of perfectly right-angled triangles, but is slightly skewed to the right. This made matters a little tricky.

## Some basics first

For those who know how to construct triangles in CSS, feel free to skip this particular section.

Let's begin.

In CSS, we create a triangle by manipulating the borders of a div with `width: 0` and `height: 0`.

Really basic stuff now. Here's a div with a height and width of 200px, and a background colour, coral.

![Making a CSS Triangle](/img/csstri1.png)

Let's apply a border of 12px.

![Making a CSS Triangle](/img/csstri2.png)

And watch what happens once we we make every border a different colour.

![Making a CSS Triangle](/img/csstri3.png)

We're going to leverage those diagonal separations in the corners. Let's make the div sizeless by making height and width 0, and increase border width to 100px.

![Making a CSS Triangle](/img/csstri4.png)

And finally, make every border (apart from the bottom one) transparent in colour.

![Making a CSS Triangle](/img/csstri5.png)

Boom. A triangle.

## Recreating the logo

*[Note: You mostly wouldn't want to recreate the Zoomo logo identically. So instead of going into the details of the **exact** dimensions of the logo and how they were recreated, I'll go over the techniques we used to make this happen.]*

In case you haven't figured out already, the Zoomo logo has two obtuse angled triangles. Constructing a triangle with altitude *x* and base *2x* was a piece of cake. The Zoomo triangles aren't so simple though. Let's reimagine each Zoomo triangle as a CSS triangle, like this.

![Making the Zoomo Spinner](/img/logo1.png)

There are a few ways you can go about this. One way is to directly compare this with the logo and adjust your border-widths accordingly. Another way is to measure the dimensions of the triangle, and mathematically derive the altitude of that triangle. The altitude will be the border-width. The latter is generally more time consuming but more accurate.

Next, rotate the div with `transform: rotate()` to the point that the base is perfectly horizontal.

![Making the Zoomo Spinner](/img/logo2.png)

And we've made the first Zoomo triangle. The second Zoomo triangle is the same thing, but inverted. Constructing this should be pretty obvious from the first one.

Position the two fins apart so that they mimic the logo. We chose to make a wrapper div, and gave it a `position: relative`. For the triangles, we chose one div and styled their `before` and `after` pseudo elements for nice, clean markup (maybe we'll make the whole thing in one div someday). Of course, you can choose to do this with two different divs. Position the two triangles with `position: absolute`.

## Creating the collapse

There are broadly two states in the animation.

* **Exploded** - triangles are apart, and are obtuse angled.  
![Zoomo Spinner exploded](/img/spinner-exploded.png)

* **Collapsed** - triangles are together, and are right angled. They form a perfect rectangle.  
![Zoomo spinner collapsed](/img/spinner-collapsed.png)

To reduce the angle to 90deg, all you need to do is reduce the border-top-width. You can obtain the exact number by some basic trigonometry, or you can do some trial-and-error &#8212; whatever you're comfortable with. The magic of this lies in the fact that the base is unaffected. The angle is *directly* controlled by the `border-top-width` only and nothing else.

![Making the Zoomo Spinner](/img/logo3.png)

## Bringing it together

First, remove any rotation applied to the individual triangles. Rotate the parent element by the amount necessary to make both triangle bases horizontal (in my case it was *35.9deg*). We'll now leave this untouched and work only on the triangles.

Let's settle on a "default" state. We found it easy to make the "collapsed" state the default one and start with that.

There are a few stages in our animation for the triangles. Throughout these stages, the only properties that change are `border-top-width` (`border-bottom-width` for the other triangle) and `transform: translate(x, y)`

*[Note: avoid animating properties such as `top`, `left`, etc. or `margin` and `padding` and most others to the extent that's possible. These are not as performant as `transform` or `opacity`. <a href="http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/" target="_blank">More information here</a>]*

These are the stages for animating the first triangle. We need to use CSS keyframes for these and basic knowledge of keyframes is required for understanding this.

* **0% to 28%** - increasing `border-top-width` and shifting the position of the triangle to the exploded state (adjust the translate property's x and y offsets for this)
* **28% to 56%** - maintaining this state while the parent wrapper element rotate animation occurs
* **56% to 84%** - returning to the collapsed state by reversing the first step
* **84% to 100%** - maintaining the collapsed state for some time

We'll leave you to the second one as it's exactly the inverse of the first.

## The spin

This part's pretty easy. The spinning animation works indepently from, but in sync with, the collapsing-exploding one. While we apply the collapsing-exploding animation to the individually to each triangle, the spin animation is applied to the parent wrapper element. All we need to do is spin the div in the state when the two triangles are maintaining their exploded state (28% to 56%). The keyframes can be broken down like this:

* **0% to 28%** - the wrapper rotation is maintained (this won't be 0, as we mentioned earlier, as we've rotated the parent to make the triangle bases horizontal)
* **28% to 56%** - spin the wrapper by 180deg
* **56% to 100%** - maintain the wrapper rotation

We don't really need to return to the original state at 0% as spinning the logo by 180deg is identical to the state at 0deg.

## Those final touches

It's imperative that the three animations (one for each triangle, and one for the wrapper) are in sync. For easings, we went with the usual `ease` but you can always experiment with other easings with `cubic-bezier` or get creative at <a href="http://cubic-bezier.com/" target="_blank">cubic-bezier.com</a>. For us, `ease` works out just fine.

Armed with these techniques, we hope you'll get cracking on something even better! If you do make something, feel free to post it in the comments. We'd love to have a look.

Thanks for reading.
