# animated-lazy-pages

It's as if [`<iron-lazy-pages>`](https://github.com/TimvdLippe/iron-lazy-pages) and [`<neon-animated-pages>`](https://elements.polymer-project.org/elements/neon-animation) had a baby.

## Features

`animated-lazy-pages` is pretty much similar to `iron-lazy-pages`. It adds the pages to the DOM only once selected.
In the same way dom-repeat uses CSS to hide show templates already added to the DOM, `animated-lazy-pages` will add `display: none` to hide the pages.
You can override this behavior with `restamp`, `animated-lazy-pages` will them remove from the DOM and add again the pages when selected/deselected

### Animating pages

All the lazy-loading logic is explained here [`<iron-lazy-pages>`](https://github.com/TimvdLippe/iron-lazy-pages), so we'll jump to animating

On each page change, `animated-lazy-pages` will grab the animation target of the previous and current page and play the animations.
It means that in your pages templates, you will need to define an animation target with the `animation-target` attribute, and this animation target must have the `neon-animatable-behavior` behavior.
You can either use `neon-animatable` or create your own page element that will have the `neon-animatable-behavior`

Example:
```html
<animated-lazy-pages attr-for-selected="name" selected="{{page}}" animate-initial-selection>
  <template is="animated-lazy-page" name="awesome">
    <neon-animatable entry-animation="fade-in-animation" animation-target>My awesome page</neon-animatable>
  </template>
</animated-lazy-pages>
```
In the above example, when `page` is set to `awesome`, `animated-lazy-pages` will play its animation.
note that we use `animate-initial-selection` to trigger the animation even when the page is selected for the first time.


## transition between pages

When switching between pages, an `exit` animation on the previous page and an `entry` animation will be played.

Example
```html
<animated-lazy-pages attr-for-selected="name" selected="{{page}}" animate-initial-selection>
  <template is="animated-lazy-page" name="awesome">
    <neon-animatable entry-animation="fade-in-animation" exit-animation="fade-out-animation" animation-target>My awesome page</neon-animatable>
  </template>
  <template is="animated-lazy-page" name="still-awesome">
    <neon-animatable entry-animation="fade-in-animation" exit-animation="fade-out-animation" animation-target>Whoa, another awesome page</neon-animatable>
  </template>
</animated-lazy-pages>
```
In the above example, switching between pages will be sweet sweet for the eyes as the `fade-in-animation` on the entering page and the `fade-out-animation` on the leaving page will be played at the same time.
If you don't believe me, check out the demos.

This is not enough as you will probably want to play different animations as you go to or come from a specific page.
Fear not, for `animated-lazy-pages` is with you!
By default, `animated-lazy-pages` will generate animation names based of the `attr-for-selected` attribute on the `animated-lazy-page`.

Example:

```html
<animated-lazy-pages attr-for-selected="name" selected="{{page}}" animate-initial-selection>
  <template is="animated-lazy-page" name="awesome">
    <neon-animatable entry-animation="fade-in-animation" exit-animation="fade-out-animation" animation-target>My awesome page</neon-animatable>
  </template>
  <template is="animated-lazy-page" name="still-awesome">
    <neon-animatable entry-animation="fade-in-animation" exit-animation="fade-out-animation" animation-target>Whoa, another awesome page</neon-animatable>
  </template>
</animated-lazy-pages>
```

Here, when switching from `awseome` to `still-awesome`, `animated-lazy-pages` will play the `to-still-awesome` animation on the animation target of the `awesome` page.
With the same logic, the `from-awesome` animation will be played on the animation target of the `still-awseome` page. As a fallback, the `exit` and `entry` animations will be played if the `to-still-awesome` or `from-awesome` animations are not configured on the animation targets.
