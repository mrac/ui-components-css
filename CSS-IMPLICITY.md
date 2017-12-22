# Component isolation

* [JavaScript/HTML](CSS-IMPLICITY.md#javascripthtml)
* [CSS inheritance](CSS-IMPLICITY.md#css-inheritance)
    * [CSS inheritance - solution](CSS-IMPLICITY.md#css-inheritance---solution)
* [CSS positioning](CSS-IMPLICITY.md#css-positioning)
    * [CSS positioning - solution](CSS-IMPLICITY.md#css-positioning---solution)
* [Basic setup](CSS-IMPLICITY.md#basic-setup)

According to best coding practices UI-components should:
- have no side-effects
- not be affected implicitly
- have only explicit dependencies

When component is not really isolated and may be affected by anything, it prevents scalability. While application grows and becomes more complex, implicit dependencies complicate testing and debugging, make it harder to understand and modify any part of code without introducing bugs elsewhere.

## JavaScript/HTML

The problem is fairly well solved within `JavaScript`/`HTML` due to modern component frameworks like React and Angular 2+, where components' state is isolated and may be set only through API (parameters/attributes). In contrast: it was fairly problematic in Angular 1 as `controller scope` was implicitly inherited from the parent `controller scope`.

## CSS inheritance

But in `CSS` there is still a problem, due to inheritance:
* https://developer.mozilla.org/en-US/docs/Web/CSS/inheritance
* https://developer.mozilla.org/en-US/docs/Web/CSS/inherit
* W3C CSS 2.1 - inherited properties - https://www.w3.org/TR/CSS21/propidx.html

Whenever CSS property is not set for properties `inherited` by default, or set to `inherit`, it will take the computed value from the parent. Components layout and look may be affected implicitly which is probably not even clearly predicted by component-library developers, let alone by its users.

### CSS inheritance - solution

This **implicity** may be solved in a simple way, thanks to `all` property:
* https://developer.mozilla.org/en-US/docs/Web/CSS/all

Setting `all: initial` in component root element reset all properties to its initial values and prevent CSS inheritance. Then the only way to set the component CSS is to target the component's selector in its parent/ancestor and explicitly set CSS. It's yet far from having the [CSS API](CSS-API.md) but very helpful already with isolation is place.

We should be conscious though, because value `initial` is the default value specified for the `CSS` property, not the default value of the `CSS` property for a given `HTML` tag. For example:
```css
div {
    display: initial;
}
```
will set computed value `display` to `inline` for all `div` tags.

As it may not be clearly expected, it would make sense to always set `display` property explicitly in the root element:

Small demo (uncomment `all: initial` and see the difference):
* https://codepen.io/anon/pen/WXLLLg

Unfortunately no IE/Edge support for that:
* https://caniuse.com/#feat=css-all

## CSS positioning

Similarly, what may implicitly affect component layout is positioning. If component
has `position:absolute` elements but doesn't define a positioning context for them (e.g. set `position:relative` somewhere upper in hierarchy), the context will be implicitly defined by the environment where the component is placed in.

### CSS positioning - solution

Fortunately that case seems to be more obvious. A good practice would be to ALWAYS set `position:relative` for component's root element, as default (any other value different than default `position:static` would serve the purpose).

Small demo below (uncomment `width: 200px` and see how positioning of some component's internals is based on the context of its parent. Then uncomment `position:relative` to fix that):
* https://codepen.io/mrac/pen/ypJegJ

This will set the positioning context for component's internals. Not only for 2 dimensions (`top`, `bottom`, `left`, `right`) but also 3D positioning, as `z-index` is a number that is interpreted as relative to its stacking context:
* https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context

## Basic setup

Example of basic setup for the component's root element:

```css
.Component {
    all: initial;
    display: block;
    position: relative;
}
```
