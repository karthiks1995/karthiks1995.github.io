---
layout: post
title:  "AngularJS-Scroll performance enhancement"
date:   2015-06-3 23:25:05
author: Karthik
categories: angularjs scroll infinitescroll bindonce lazy 
permalink: "/angularjs-infinite-scroll-performance-enhanced"
image: true
---

AngularJS is a popular web application framework maintained by Google and several other open source communities for developing dynamic web apps. It enhances HTML by attaching directives to your pages with new attributes or tags and expressions in order to define very powerful templates directly in your HTML. If you are thinking why AngularJS, why not JSP or FTL, this article <a href="http://jeffwhelpley.com/angularjs/">Why AngularJS?</a> gives the right reasons.

If you want to brush up your AngularJS knowledge, use these tutorials.

* <a href="https://docs.angularjs.org/guide/introduction">AngularJS Developer Guide<a>
* <a href="http://www.w3schools.com/angular/">AngularJS Tutorial<a>

Iterations are one of the basic requirements in any language or framework. Popular languages like C, C++, Java etc use loops like `for` and `while`. `ng-repeat` provides the basic iterating mechanism in AngularJS.

Data binding is the core concept of AngularJS. It is automatic synchronization of data between model and view components. The view is a projection of model always, so whenever the model changes, view changes and vice versa. Two basic terms associated with AngularJS Data binding are  `$watch` and `$digest`.

When you bind a value to an element in AngularJS, it creates `$watch` on that value. Thereafter, whenever a value on a scope changes, all `$watch`es observing that value are executed to update the changed value. A `$digest` cycle is an internal execution loop that runs through your entire application's binding and checks if any of the value needs to be updated. Increase in number of bindings, increases number of `$watch`es,thereby increasing time required to process each `$digest` cycle.
The key concept behind performance enhancement in AngularJS is reducing number of `$watch`es which improves `$digest` cycle's performance. 

If you want to have an infinite scroll in your website (here infinite meaning large number of entries like 800-1000 entries per page), you cannot call `ng-repeat` on the whole array of 800 entries as this would take a serious toll on the performance resulting in a slow website because of two reasons :

* `ng-repeat` slowness creates an overhead.
* Increase in entries will also increase `$watch`es thereby reducing performance.

We use lazy approach to solve the first problem and the concept of bindonce to solve the second problem.

##The Lazy Approach

The compile phase of the AngularJS is one thing that decides your webpage's performance. Your AngularJS code is compiled to form the view html which is then displayed by the browser. For example, if you have a `ng-repeat` inside `<div>` iterated 10 times, upon compilation it creates ten `<div>` tags in the view html, and for each tag here, `$watch`ers are created. So, if you are loading large number of entries, compiler would have have to create that many tags which would take a long time resulting in a slow page. The multiplicative increase in the number of `$watch`ers is also an issue. These are the main causes of `ng-repeat` slowness.

This problem can be solved by Lazy approach. Initially we load a small number of entries and display them, as and when the user scrolls down, further entries are loaded and displayed. This reduces the overhead of compiling large number of tags initially, thus enhances the performance of your website. 

For example, if you want to display an array of 2000 entries, initially around 40 entries are loaded. When the user scrolls down and reaches the near end of the bar, another 40 entries are loaded.Take a look at <a href="http://www.gozoomo.com/bangalore">www.gozoomo.com/bangalore</a> and scroll down to experience the use of lazy approach. As and when you reach the end of the scroll bar, new cars are loaded.
 
<!--The code below shows the implementation of the lazy-approach.
{% gist karthiks1995/1e8f87836961bb3a8b4c %}-->

##Bindonce

To have a responsive, lag-free and beautiful webpage, number of `$watch`es should be less than a thousand. This number can easily exceed if proper care is not taken during data binding. AngularJS internally creates a `$watch` for each `ng-*` directive in order to keep the values updated. But, if the entries that are being loaded are static, all the `$watch`ers created are unnecesary. These can be avoided by using `bindonce`. Bindonce is a set of directives meant for bindings that are not going to change while the user is on a page. We can change all the interpolations `{{....}}` like these to `bo-text`. Most of `ng-*` can be converted to bindonce by simply changing them to `bo-*`. <a href="https://github.com/Pasvaz/bindonce">This</a> repo gives more details about using the concept of bindonce.

AngularJS is a huge framework with many performance enhancements already built in it, but still it cannot solve all our problems. Understanding the key concepts and avoiding bad practices can certainly optimise the performance of your site. For any information on AngularJS see <a href="https://docs.angularjs.org/api">documentation</a>.








