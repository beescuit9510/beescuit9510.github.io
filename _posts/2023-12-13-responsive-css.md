---
title: Responsive CSS
date: 2023-12-13 12:00:00 -500
categories: [frontend, css]
tags: [frontend, css, responsive] # TAG names should always be lowercase
---

# Responsive CSS

CSS is the most basic skill in web development, but working with it can still be daunting and tricky. I still find myself struggling with CSS at work as well. However, I recently came across [Conquering Responsive Layouts](https://courses.kevinpowell.co/conquering-responsive-layouts) by Kevin Powell, and I've learned so much from the tutorial.

In this article, I want to share some of the lessons and tips that I've learned from the tutorial.

## Table of Contents

- [Our website IS responsive by default](#our-website-is-responsive-by-default)
- [Percentage for Width](#percentage-for-width)
  - [Fixed Units](#fixed-units)
  - [Why We Should Not Use `overflow: hidden;`](#why-we-should-not-use-overflow-hidden)
- [Avoiding Height](#avoiding-height)
- [More Relative Units: `em` and `rem`](#more-relative-units-em-and-rem)
  - [`em`](#em)
  - [`rem`](#rem)
  - [_`rem`_ and _`em`_ for Width, Height, Padding, Margin, and More](#rem-and-em-for-width-height-padding-margin-and-more)
  - [`em` and `rem` with Media Queries](#em-and-rem-with-media-queries)
  - [When to Use Which One?](#when-to-use-which-one)
- [More Relative Units: `vh`, `vw`, `vmin`, `vmax`](#more-relative-units-vh-vw-vmin-vmax)
  - [Quick Intro to `vh`, `vw`, `vmin`, `vmax`](#quick-intro-to-vh-vw-vmin-vmax)
  - [Usecase](#usecase)

## Our website IS responsive by default

One thing to realize is that our website is responsive by default. This might sound far from the truth. However, _**our website is indeed responsive by default unless we compromise its responsiveness with additional CSS.**_ If we do not have any CSS, our website would look ugly but still be responsive. It is we who cause problems for ourselves.

For example, if we do not set a `height` or `width` for an element, the `width` and `height` change based on the screen size since block-level elements have `width: 100%` by default. The problem arises when we set fixed widths for both height and width.

```css
%%
  we
  will
  have
  side
  scrollbar
  once
  our
  screen
  is
  smaller
  than
  900px
  %%
  .example {
  width: 900px;
}
```

When we think of responsiveness, `media queries` are the first thing that comes to mind. However, we know that our website is responsive by default. This means that _our websites can be both good-looking and responsive without the need for `media queries`_ (although we still need media queries, but we can minimize the use).

## Percentage for Width

### Fixed Units

As mentioned earlier, it is **us** who make our website anti-responsive. So, what is the most common mistake we make?

**_The most common mistake is setting a `width` or `height` with fixed units_** (or [Absolute Lengths units](https://www.w3schools.com/css/css_units.asp)). When we use fixed units, we end up with a side scrollbar when the screen size is smaller than the fixed size, and overflow occurs when the element cannot fit inside the parent, causing it to overflow beyond its parent.

```html
<body>
  <div class="parent">
    <div class="child"></div>
  </div>
</body>
```

```css
.parent {
	margin: 0 auto;
	width: 80%;
}
.child {
	%% this will potentially overfill the parent. %%
	width: 750px;
}
```

### Why We Should Not Use overflow hidden

Overflow is one of the features that can be frustrating, but it is a part of CSS's nature. CSS does its best to ensure that users do not lose any information, so it chooses overflow instead of hiding the information. This approach helps us avoid potentially creating a bigger problem in the future where we unintentionally lose important information.

So technically, we can fix overflow with `overflow: hidden;`, but this goes against what CSS intends, potentially causing bigger problems for ourselves in the future.

So how do we fix such a problem without `overflow:hidden`?

We can simply make use of `percentage`(or [Relative length units](https://www.w3schools.com/css/css_units.asp))

```css
.child {
	%% percentage is relative to the parent element. %%
	width: 80%;
}
```

## Avoiding Height

Now, we understand how to deal with `width`. But what about `height`?

We can say that `width` is much more manageable than `height`. When we set a `height`, overflow can occur depending on the length of its content and the browser size and we might also accidentally lose information as well.

```css
.container {
	%% the content might or might not overflow depending on the screen size and content length%%
	height: 300px;
}
```

So, setting a `height` seems like more pain than gain. Once again, our website is responsive by default. It is **us** that causes problems for ourselves. So, _**why don't we simply take the height away? The height of the block-level elements grows and shrinks depending on the length of the content by default.**_

## More Relative Units: em and rem

`Percentage` is very useful when it comes to `width`. What about other relative units?

### em

`em` is relative to the element's parent's font size.

```css
.parent {
	font-size: 10px;
}
.child {
	%% 10(parent's font size)*2.5%%
	font-size: 2.5em
}
```

One thing to be aware of is that **`em` units can easily be out of control due to their compounding nature**. They can become significantly larger than expected, especially when multiple `em` font-sized elements are nested within one another.

Ironically, we might want to avoid using `em` for `font-size`.

```css
.parent {
	%% most of browsers default font-size is 16px %%
	font-size: 2em %% 16*2 %%
}
.child {
	font-size: 2.5em %% (16*2)*2.5 %%
}
```

### rem

`rem` can help us with this compounding problem. The **_r_** in `rem` stands for root. As the name suggests, `rem` is relative to the font size of the root element(`<html>`). thus, consistent!

### rem and em for Width, Height, Padding, Margin, and More

We can use `rem` and `em` not only for `font-size` but also for `width`, `height`, `padding`, `margin`, or any properties that take units.

One thing to be aware of is that _when we use `em` with any properties other than `font-size`, the property's value is basically relative to the font-size of its own element._

```css
.btn {
  font-size: 10px;
  padding: 0.75em 2em; %% 10*.75 10*2%%
}
```

This can be confusing compared to `rem`, which is always relative to the root element. However, it can also be useful, especially for `padding`, because `padding` will be automatically adjusted to the font-size of its element without additional CSS.

```css
button {
	padding: 0.75em 3em;
}
.btn-large {
	font-size: 10px %% padding: 10*0.75 10*3; %%
}
.btn-small {
	font-size: 5px; %% padding: 5*0.75 5*3; %%
}
```

### em and rem with Media Queries

When we use `rem` and `em` for `width`, `height`, `padding`, `margin`, and other properties, they naturally adjust when font-size changes. This can be very useful with `media queries`, allowing us to adjust multiple parts of the entire site very easily.

```css
html {
  font-size: 16px;
}

@media (min-width: 700px) {
  html {
    font-size: 25px;
  }
}
```

#### When to Use Which One

Simple! **_For consistent spacing based on the root element, use `rem`. For spacing based on `font-size` of an element, use `em`._**

## More Relative Units: vh, vw, vmin, vmax

Another set of useful relative units is `vh`, `vw`, `vmin`, and `vmax`. Here, _v_ stands for viewport, and as their names suggest, they are all relative to the viewport. We can think of the viewport as _the area of the web page that is visible to the user in their browser window._

### Quick Intro to vh, vw, vmin, vmax

- `vh` is relative to the height of viewport
- `vw` is relative to the width of viewport
- `vmin` is relative to the min of `vw` and `vh`
- `vmax` is relative to the max of `vw` and `vh`

### Usecase

They are not as often used as `percentage`. but, they can still come in handy, one of the known example is **_setting `font-size` with `vmin` or `vw` unit for titles_**

**_If we use `vmin` or `vw` for `font-size`, whenever we increase or decrease the size of our browser, `font-size` also dose along with the browser._** this helps us write so much less `media queries`. even though, we still need to use `media queries` because `font-size` will be either too big or too small than necessary at some point. (this is why, we typically do not use `vw` for paragraphs.)

So they can be useful. however, working with those units is tricky. for example, we often use `vh` for `height`, this can work great but we can also have unexpected overflow in a smaller screen size.

To avoid such mistakes, we typically need extensive testing with different screen sizes and screen orientations when we work with them.
