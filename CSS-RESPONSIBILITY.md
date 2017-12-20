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

### External layout

**External layout** defines how component is placed inside its parent. It is mainly associated with **root element** of the component.

It may be set entirely IN the parent component, for example:

```html
<MyForm>
    <MyInput class="my-input-1"></MyInput>
    <MyInput class="my-input-2"></MyInput>
</MyForm>
```

```css
.MyForm {
    display: flex;
    flex-direction: row-reverse;
    justify-content: center;
    align-items: flex-end;
    padding: 50px 20px;
}
```

Or may be set in the root element of the component BY the parent component, for example:

```css
.MyForm {
    display: block;
}
.MyForm .my-input-1 {
    display: inline;
    margin: 10px 50px;
    width: 100px;
}
.MyForm .my-input-2 {
    display: inline;
    margin: 10px 30px;
    width: 200px;
}
```

In both cases however it looks like the component itself should never be responsible for setting its own **external layout**. This responsibility should be left to its ancestors.

Nevertheless default values may be preset:

```css
.MyInput {
    display: block;
    width: 100%;
}
```

### Internal layout

**Internal layout** defines how component internal elements are placed within component's own space. Those CSS settings may be set up in the root element of the component and in the descendants. The descendants may also be custom components so in another words: it's about setting their external layout CSS.

It is the responsibility of the component to set up its own internal layout.

However, component's parent/ancestors may want to overwrite/customise it.

### Display property

`display` property is tricky, because it may be used to set both internal and external layout on the root element of the component, for example:

* `display:flex` - external->`block`, internal->`flex`
* `display:inline-flex` - external->`inline`, internal->`flex`
* `display:block` - external->`block`, internal->`block`
* `display:inline-block` - external->`inline`, internal->`block`

Therefore, whenever a component set the external-layout of its children, it may overwrite their internal-layout settings (`display` property), which may not be desirable.

### Look

For components to be useful in many contexts, it should be possible to set their look according to the application styling-theme.

**Native HTML5 elements** keep their own look-CSS minimal, setting only some defaults and leaving this responsibility to higher-level component (usually on the application-level where global CSS classes are defined).

That approach comes with many advantages:
- makes components highly reusable in applications with different styling-themes
- makes possible to switch/modify styling theme, keeping components untouched
- prevents coupling unrelated components by such trivial thing as styling ;)
- limits components' responsibility, contributing to code clarity and scalability

### Components are not responsible for its look

In the world of **custom components** this approach needs some adjustments:
- global CSS classes with visual meaning cannot be hardcoded inside components
- components may use semantic CSS classes reflecting elements functionality
- there should be a way to namespace semantic CSS classes to prevent name conflicts
- semantic CSS classes can be targeted by selectors on **appliation level**, to set their look according to **application styling theme**

Any element inside any component may be targeted by the parent using a selector which is combined of three dimensions:
* its component type class
* its component instance class (optional)
* the element's semantic class

Looking from the application root component (where application styling theme is set) the selectors may be composed of those 3 dimensions of each element on the path, reflecting the components HTML hierarchy.

For example `MyInput` component template in React:

```html
<div className={'MyInput '+ this.props.className}>
    <label className="MyInput-label">{this.props.label}</label>
    <input className="MyInput-text" type="text" value="{this.state.value}" onChange="{this.update}">
    <button className="MyInput-clear" onClick="{this.clear}">X</button>
</div>
```

would be used in the parent component `MyPerson`:

```html
<div className={'MyPerson ' + this.props.className}>
    <MyInput className="MyPerson-name" label="Name" />
    <MyInput className="MyPerson-address" label="Address" />
</div>
```

The components look is defined in the application root component `MyApp`:

```html
<div className={'MyApp'}>
    <MyPerson>
</div>
```

```css
/* MyInput */
.MyApp .MyInput .MyInput {
    font-size: 10px;
}

.MyApp .MyInput .MyInput-clear {
    background-color: lightgrey;
    color: black;
}

/* MyPerson */
.MyApp .MyPerson .MyInput.MyPerson-name {
    font-size: 15px;
}
```
