# Basic setup

Example of basic default setup for the component's root element:

```css
.Component {
    all: initial;
    display: block;
    position: relative;
    box-sizing: border-box;
}
```

* `all: initial` prevents CSS inheritance and isolates the component CSS ([see](CSS-IMPLICITY.md#css-inheritance))
* `display: block` explicitly sets `display` property, otherwise initial value would be `inline` ([see](CSS-IMPLICITY.md#css-inheritance---solution))
* `position: relative` prevents positioning `absolute` elements by outside position/stacking context ([see](CSS-IMPLICITY.md#css-positioning))
* `box-sizing: border-box` binds `width`,`height` properties only to external-layout ([see](CSS-RESPONSIBILITY.md#box-sizing)])

