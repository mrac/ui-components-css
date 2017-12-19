## Component responsibility

`CSS` is used to set components' layout and look.

### Layout

Layout is about setting the compoment's place in a web document. In old days before `CSS` has become mature technology, it was mainly accomplished by `HTML` markup (`width`, `height` attributes, excesive usage of `table` element, order of tags).

It answers the questions: *where?*, *how big?*, *how far from other components?*
We can see a web document as 3D space where components may be placed.

Layout is usually defined by the following CSS properties:
- `display`
- `position`
- `flex-*`, `align-*`, `order`, `justify-content`
- `float`, `clear`
- `left`, `right`, `top`, `bottom`
- `margin`, `border`
- `width`, `height`, `max-width`, `max-height`, `min-width`, `min-height`
- `box-sizing`
- `z-index`

There can be 2 types of layout CSS:
- external layout - how the component interracts with siblings and ancestors
- internal layout - the layout of the children within the component

#### External layout

External layout defines how component is placed inside its parent. It is mainly associated with **root element** of the component.

It may be set entirely IN the parent component, for example:

```html
<div class="container">
    <MyInput class="my-input-1"></MyInput>
    <MyInput class="my-input-2"></MyInput>
</div>
```

```css
.container {
    display: flex;
    justify-content: center;
    align-items: flex-end;
    padding: 50px 20px;
}
```

Or may be set in the root element of the component BY the parent component, for example:

```css
.container {
}
.container .my-input-1 {
    display: inline;
    margin: 10px 50px;
    width: 100px;
}
.container .my-input-2 {
    display: inline;
    margin: 10px 30px;
    width: 200px;
}
```

In both cases however it looks like the component itself is never responsible to set its own **external layout**. This responsibility is provided to its ancestors.

Nevertheless default values may be preset.
